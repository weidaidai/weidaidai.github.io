

### golang使用mysql

安装驱动

```go
"database/sql"
   "fmt"
   _ "github.com/go-sql-driver/mysql"

```

```
_ "github.com/go-sql-driver/mysql"//_下划线表示引用的时候它是一个匿名的引用
是用来忽略变量赋值的占位符，那么包引入用到这个符号也是相似的作用，这儿使用 _ 的意思是引入后面的
包名而不直接使用这个包中定义的函数，变量等资源。相当于init（）
```

前提安装gin的框架  github.com/gin-gonic/gin

####  连接数据库

```go
func Open(driverName, dataSourceName string) (*DB, error)//dataSourceName数据库的名称
```

> Open打开一个dirverName指定的数据库，dataSourceName指定数据源，一般至少包括数据库文件名和其它连接必要的信息。

```go
import (
	"database/sql"

	_ "github.com/go-sql-driver/mysql"
)

func main() {
   // DSN:Data Source Name
	dsn := "user:password@tcp(127.0.0.1:3306)/dbname"
	db, err := sql.Open("mysql", dsn)
	if err != nil {
		panic(err)
	}
	defer db.Close()  // 注意这行代码要写在上面err判断的下面，要做完错误判断保证db不为nil才可以关闭释放连接数库的资源
}
```

一般使用要进行初始化， 定义一个全局对象db

```go
package main

import (
	"database/sql"
	"fmt"
	_"github.com/go-sql-driver/mysql"
)
type user  struct {
	id   int
	age float32
	name string
}

// 定义一个全局对象db
var db *sql.DB

// 定义一个初始化数据库的函数
func initDB() (err error) {
	// DSN:Data Source Name
	dsn := "root:123456@tcp(127.0.0.1:3306)/sql_domo"//前提要创建一个数据库sql_domo
	// 不会校验账号密码是否正确
	// 注意！！！这里不要使用=，我们是给全局变量赋值，然后在main函数中使用全局变量db
	db, err = sql.Open("mysql", dsn)//数据库的名称mysql sql.Open是database/sql本地
	if err != nil {
		return err
	}
	// 尝试与数据库建立连接（校验dsn是否正确）
	err = db.Ping()
	if err != nil {
		fmt.Printf("connect to db failed,err:%v\n",err)
		return err
	}
	//根据业务范围确定最大连接数
	db.SetMaxIdleConns(10)
	return nil
}
// 查询多条数据示例
func queryMultiRowDemo() {
	sqlStr := "select id, name, age from user where id > ?"
	rows, err := db.Query(sqlStr, 0)
	if err != nil {
		fmt.Printf("查询失败, err:%v\n", err)
		return
	}
	// 非常重要：关闭rows释放持有的数据库链接
	defer rows.Close()

	// 循环读取结果集中的数据
	for rows.Next() {
		var u user
		err := rows.Scan(&u.id, &u.name, &u.age)
		if err != nil {
			fmt.Printf("scan failed, err:%v\n", err)
			return
		}
		fmt.Printf("id:%d name:%s age:%f\n", u.id, u.name, u.age)
	}
}
func main() {
	if err := initDB();
		err != nil {
		fmt.Println("初始化连接失败",err)
		return
	}
	fmt.Println("连接成功")
	defer db.Close()
	initDB()
	queryMultiRowDemo()
}
```

根据业务范围确定

db.setmaxopenconns(100)//最大连接数100

db.setmaxidleconns(10)//最大空闲连接数10

#### 查询

为了方便查询，我们事先定义好一个结构体来存储user表的数据。

```go
type user struct {
	id   int
	age  int
	name string
}
```

#### 单行查询

> 单行查询`db.QueryRow()`执行一次查询，并期望返回最多一行结果（即Row）。QueryRow总是返回非nil的值，直到返回值的Scan方法被调用时，才会返回被延迟的错误。（如：未找到结果）

```go
func (db *DB) QueryRow(query string, args ...interface{}) *Row
```

代码

```go
//查询单条数据
func queryRowDemo() {
	sqlStr := "select id, name, age from user where id=?"
	var u user//user结构体赋值给u
	// 非常重要：确保QueryRow返回的row对象，之后调用Scan方法，否则持有的数据库链接不会被释放
    //row:=db.db.QueryRow(sqlStr, 2)//这个后续没有调用Scan的方法会导致程序一直处于连接状态，程序崩溃
	row := db.QueryRow(sqlStr, 1)//把sqlstr字符串的语句传输进去后，1表示第一行
	err:=row.Scan(&u.id, &u.name, &u.age)
	if err != nil {
		fmt.Printf("scan failed, err:%v\n", err)
		return
	}
	fmt.Printf("id:%d name:%s age:%d\n", u.id, u.name, u.age)
}//id：1 name：dongdong age18
```

#### 多行查询

> 多行查询`db.Query()`执行一次查询，返回多行结果（即Rows），一般用于执行select命令。参数args表示query中的占位参数。

```go
func (db *DB) Query(query string, args ...interface{}) (*Rows, error)
```



```go
// 查询多条数据示例
func queryMultiRowDemo() {
    sqlStr := "select id, name, age from user where id > ?"
    rows, err := db.Query(sqlStr, 0)
    if err != nil {
        fmt.Printf("query failed, err:%v\n", err)
        return
    }
    // 非常重要：关闭rows释放持有的数据库链接
    defer rows.Close()

    // 循环读取结果集中的数据
    for rows.Next() {
        var u user
        err := rows.Scan(&u.id, &u.name, &u.age)
        if err != nil {
            fmt.Printf("scan failed, err:%v\n", err)
            return
        }
        fmt.Printf("id:%d name:%s age:%d\n", u.id, u.name, u.age)
    }
}
//id：1 name：dongdong age18
//id：1 name：xiaodong age28
```

main函数

```go
func main(){
    if err:=initmysql();err!=nil{
        fmt.Println("connect to db failed",err)
    }
    defer db.Close()
    fmt.Println("connect to db success")
    //单行查询
    queryRowDemo()
    // 查询多条数据示例
    queryMultiRowDemo() 
    
}
```



### 插入数据

插入、更新和删除操作都使用方法。

```go
func (db *DB) Exec(query string, args ...interface{}) (Result, error)
```

Exec执行一次命令（包括查询、删除、更新、插入等），返回的Result是对已执行的SQL命令的总结。参数args表示query中的占位参数。

具体插入数据示例代码如下：



```go
// 插入数据
func insertRowDemo() {
    sqlStr := "insert into user(name, age) values (?,?)"//insert sql语句 
    ret, err := db.Exec(sqlStr, "王五", 38)
    if err != nil {
        fmt.Printf("insert failed, err:%v\n", err)
        return
    }
    theID, err := ret.LastInsertId() // 新插入数据的id
    if err != nil {
        fmt.Printf("get lastinsert ID failed, err:%v\n", err)
        return
    }
    fmt.Printf("insert success, the id is %d.\n", theID)
}
```

 

### 更新数据

具体更新数据示例代码如下：

```go
// 更新数据
func updateRowDemo() {
    sqlStr := "update user set age=? where id = ?"
    ret, err := db.Exec(sqlStr, 39, 3)//将新插入第三行3的38岁改为39岁
    if err != nil {
        fmt.Printf("update failed, err:%v\n", err)
        return
    }
    var n int64
    n,err= ret.RowsAffected() // 操作影响的行数
    if err != nil {
        fmt.Printf("get RowsAffected failed, err:%v\n", err)
        return
    }
    fmt.Printf("update success, affected rows:%d\n", n)
}
//update success, affected rows1
```

### 删除数据

具体删除数据的示例代码如下：

```go
// 删除数据
func deleteRowDemo() {
    sqlStr := "delete from user where id = ?"
    ret, err := db.Exec(sqlStr, 3)
    if err != nil {
        fmt.Printf("delete failed, err:%v\n", err)
        return
    }
    n, err := ret.RowsAffected() // 操作影响的行数
    if err != nil {
        fmt.Printf("get RowsAffected failed, err:%v\n", err)
        return
    }
    fmt.Printf("delete success, affected rows:%d\n", n)
}
```

### MySQL预处理

什么是预处理？

> 普通SQL语句执行过程：

1. 客户端对SQL语句进行占位符替换得到完整的SQL语句。
2. 客户端发送完整SQL语句到MySQL服务端
3. MySQL服务端执行完整的SQL语句并将结果返回给客户端。

> 预处理执行过程：

1. 把SQL语句分成两部分，命令部分与数据部分。
2. 先把命令部分发送给MySQL服务端，MySQL服务端进行SQL预处理。
3. 然后把数据部分发送给MySQL服务端，MySQL服务端对SQL语句进行占位符替换。
4. MySQL服务端执行完整的SQL语句并将结果返回给客户端。

#### 为什么要预处理？

1. 优化MySQL服务器重复执行SQL的方法，可以提升服务器性能，提前让服务器编译，一次编译多次执行，节省后续编译的成本。（频繁对数据进行操作）
2. 避免SQL注入问题。

#### Go实现MySQL预处理

Go中的

```go
func (db *DB) Prepare(query string) (*Stmt, error)
```

`Prepare`方法会先将sql语句发送给MySQL服务端，返回一个准备好的状态用于之后的查询和命令。返回值可以同时执行多个查询和命令。

查询操作的预处理示例代码如下：

```go
// 预处理查询示例
func prepareQueryDemo() {
    sqlStr := "select id, name, age from user where id > ?"
    stmt, err := db.Prepare(sqlStr)
    if err != nil {
        fmt.Printf("prepare failed, err:%v\n", err)
        return
    }
    defer stmt.Close()
    //先提前编译
    rows, err := stmt.Query(0)
    if err != nil {
        fmt.Printf("query failed, err:%v\n", err)
        return
    }
    defer rows.Close()
    // 循环读取结果集中的数据
    for rows.Next() {
        var u user
        err := rows.Scan(&u.id, &u.name, &u.age)
        if err != nil {
            fmt.Printf("scan failed, err:%v\n", err)
            return
        }
        fmt.Printf("id:%d name:%s age:%d\n", u.id, u.name, u.age)
    }
}
```

> 插入、更新和删除操作的预处理十分类似，这里以插入操作的预处理为例：

```go
// 预处理插入示例
func prepareInsertDemo() {
	sqlStr := "insert into user(name, age) values (?,?)"
	stmt, err := db.Prepare(sqlStr)
	if err != nil {
		fmt.Printf("prepare failed, err:%v\n", err)
		return
	}
	defer stmt.Close()
    //先提前编译
	_, err = stmt.Exec("小王子", 18)//只需要调用stmt这个方法即可插入
	if err != nil {
		fmt.Printf("insert failed, err:%v\n", err)
		return
	}
	_, err = stmt.Exec("小美", 18)//只需要调用stmt这个方法即可插入
	if err != nil {
		fmt.Printf("insert failed, err:%v\n", err)
		return
	}
	fmt.Println("insert success.")
}
//id：1 name：dongdong age18
//id：2 name：xiaodong age28
//id:3 name：王五 age39
//id:4 name：小王子 age18
//id:5 name：小妹 age18
```

