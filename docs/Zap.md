### Zap 的 Hello World

 代码大概长下面这样：

```go
package main

import (
	"fmt"
	"go.uber.org/zap"
)

func main() {
	var logger *zap.Logger
	logger, _ = zap.NewProduction()
	logger.Debug("i am debug")   // 这行不会打印，因为默认日志级别是 INFO
	logger.Info("i am info")     // INFO  级别日志，这个会正常打印
	logger.Warn("i am warn")     // WARN  级别日志，这个会正常打印
	logger.Error("i am error")   // ERROR 级别日志，这个会打印，并附带堆栈信息
	logger.Fatal("i am fatal")   // FATAL 级别日志，这个会打印，附带堆栈信息，并调用 os.Exit 退出
	fmt.Println("can i be printed?")  // 这行不会打印，呃...上面已经退出了
}

```

```go
{"level":"info","ts":1608362501.672074,"caller":"zap2/main.go:11","msg":"i am info"}
{"level":"warn","ts":1608362501.672139,"caller":"zap2/main.go:12","msg":"i am warn"}
{"level":"error","ts":1608362501.672147,"caller":"zap2/main.go:13","msg":"i am error","stacktrace":"main.main\n\t/Users/jack/go/src/helloworld/zap2/main.go:13\nruntime.main\n\t/usr/local/Cellar/go/1.15.2/libexec/src/runtime/proc.go:204"}
{"level":"fatal","ts":1608362501.6721768,"caller":"zap2/main.go:14","msg":"i am fatal","stacktrace":"main.main\n\t/Users/jack/go/src/helloworld/zap2/main.go:14\nruntime.main\n\t/usr/local/Cellar/go/1.15.2/libexec/src/runtime/proc.go:204"}

```

以上代码应该是相当简洁易懂了，在这里需要再唠嗑一下的是关于输出的日志的几个字段的说明：

level: 顾名思义，就是日志的级别了，默认的日志级别是 INFO，所以我们的第一行日志没有被打印出来
ts: timestamp，时间戳的意思
caller: 调用者，就是打印日志的那行代码的位置
stacktrace: 调用堆栈，及打印日志的那行代码所在的堆栈信息，默认 ERROR 级别及以上的日志会附带堆栈信息，可以修改

### case 2: SugaredLogger

Sugar 是糖的意思，顾名思义，zap.SugaredLogger 就是对 zap.Logger 进行了封装，提供了一些高级的语法糖特性，如支持使用类似 fmt.Printf 的形式进行格式化输出，使用 zap.SugaredLogger 改造一下上面的例子

```go
package main

import (
	"fmt"
	"go.uber.org/zap"
)

func main() {
	var sugaredLogger *zap.SugaredLogger
	logger, _ := zap.NewProduction()
	sugaredLogger = logger.Sugar()
	sugaredLogger.Debugf("i am debug, using %s", "sugar")   // 这行不会打印，因为默认日志级别是 INFO
	sugaredLogger.Infof("i am info, using %s", "sugar")      // INFO  级别日志，这个会正常打印
	sugaredLogger.Warnf("i am warn, using %s", "sugar")    // WARN  级别日志，这个会正常打印
	sugaredLogger.Errorf("i am error, using %s", "sugar")   // ERROR 级别日志，这个会打印，并附带堆栈信息
	sugaredLogger.Fatalf("i am fatal, using %s", "sugar")    // FATAL 级别日志，这个会打印，附带堆栈信息，并调用 os.Exit 退出
	fmt.Println("can i be printed?")  // 这行不会打印，呃...上面已经退出了
}


```

```go
{"level":"info","ts":1608365481.615607,"caller":"zap3/main.go:13","msg":"i am info, using sugar"}
{"level":"warn","ts":1608365481.615681,"caller":"zap3/main.go:14","msg":"i am warn, using sugar"}
{"level":"error","ts":1608365481.6156878,"caller":"zap3/main.go:15","msg":"i am error, using sugar","stacktrace":"main.main\n\t/Users/jack/go/src/helloworld/zap3/main.go:15\nruntime.main\n\t/usr/local/Cellar/go/1.15.2/libexec/src/runtime/proc.go:204"}
{"level":"fatal","ts":1608365481.6157131,"caller":"zap3/main.go:16","msg":"i am fatal, using sugar","stacktrace":"main.main\n\t/Users/jack/go/src/helloworld/zap3/main.go:16\nruntime.main\n\t/usr/local/Cellar/go/1.15.2/libexec/src/runtime/proc.go:204"}

```

### case 3: 定制化 SugaredLogger

使用 zap.NewProduction() 实际上是创建了一个带了预置配置的 zap.Logger，我们可以使用 zap.New(core zapcore.Core, options ...Option) 来手动传递所有配置项，zapcore.Core 需要 3 个配置：

Encoder: 编码器，用于告诉 zap 以什么样的形式写入日志。zap 提供了 jsonEncoder 和 consoleEncoder 两种内置的编码器
WriterSyncer: 告诉 zap 将日志写到哪里，比如文件(os.File) 或标准输出(os.stdout)
LevelEnabler: 用于设置 zap.Logger 的日志级别
定制化

```go
package main

import (
	"fmt"
	"go.uber.org/zap"
	"go.uber.org/zap/zapcore"
	"os"
)

func main() {

	// 配置 sugaredLogger
	var sugaredLogger *zap.SugaredLogger
	writer := zapcore.AddSync(os.Stdout)  // 设置日志输出的设备，这里还是使用标准输出，也可以传一个 File 类型让它写入到文件
	encoder := zapcore.NewJSONEncoder(zap.NewProductionEncoderConfig())  // 设置编码器，即日志输出的格式，默认提供了 json 和 console 两种编码器，这里我们还是使用 json 的编码器
	core := zapcore.NewCore(encoder, writer, zapcore.InfoLevel) // 设置日志的默认级别
	logger := zap.New(core)
	sugaredLogger = logger.Sugar()

	// 打印日志
	sugaredLogger.Debugf("i am debug, using %s", "sugar")   // 这行不会打印，因为日志级别是 INFO
	sugaredLogger.Infof("i am info, using %s", "sugar")      // INFO  级别日志，这个会正常打印
	sugaredLogger.Warnf("i am warn, using %s", "sugar")    // WARN  级别日志，这个会正常打印
	sugaredLogger.Errorf("i am error, using %s", "sugar")   // ERROR 级别日志，这个会打印，并附带堆栈信息
	sugaredLogger.Fatalf("i am fatal, using %s", "sugar")    // FATAL 级别日志，这个会打印，附带堆栈信息，并调用 os.Exit 退出
	fmt.Println("can i be printed?")  // 这行不会打印，呃...上面已经退出了
}

```

编译运行该程序，输出类似如下：

```go
{"level":"info","ts":1608367820.9526541,"msg":"i am info, using sugar"}
{"level":"warn","ts":1608367820.952697,"msg":"i am warn, using sugar"}
{"level":"error","ts":1608367820.9527,"msg":"i am error, using sugar"}
{"level":"fatal","ts":1608367820.952702,"msg":"i am fatal, using sugar"}
```


和上面的相比，这里不会打印 caller 和 stackstace，因为我们没有配置，需要我们显式去配置它们。

和上面的相比，这里不会打印 caller 和 stackstace，因为我们没有配置，需要我们显式去配置它们。

### 使用 Console 格式

默认的 json 格式非常方面用于专业的第三日志分析工具（如 elk）处理，但可读性就不是非常好，如果日志是用来给人看的话，设置成 console 格式的可读性会好一点，特别是存在换行的时候（json 格式的每条日志只会显示一行），将上述栗子改成 console 格式：

默认的 `json` 格式非常方面用于专业的第三日志分析工具（如 elk）处理，但可读性就不是非常好，如果日志是用来给人看的话，设置成 `console` 格式的可读性会好一点，特别是存在换行的时候（`json` 格式的每条日志只会显示一行），将上述栗子改成 `console` 格式：

```go
package main

import (
	"fmt"
	"go.uber.org/zap"
	"go.uber.org/zap/zapcore"
	"os"
)

func main() {

	// 配置 sugaredLogger
	var sugaredLogger *zap.SugaredLogger
	writer := zapcore.AddSync(os.Stdout)
	encoder := zapcore.NewConsoleEncoder(zap.NewProductionEncoderConfig())  // 设置 console 编码器
	core := zapcore.NewCore(encoder, writer, zapcore.InfoLevel)
	logger := zap.New(core)
	sugaredLogger = logger.Sugar()

	// 打印日志
	sugaredLogger.Debugf("i am debug, using %s", "sugar")   // 这行不会打印，因为日志级别是 INFO
	sugaredLogger.Infof("i am info, using %s", "sugar")      // INFO  级别日志，这个会正常打印
	sugaredLogger.Warnf("i am warn, using %s", "sugar")    // WARN  级别日志，这个会正常打印
	sugaredLogger.Errorf("i am error, using %s", "sugar")   // ERROR 级别日志，这个会打印，并附带堆栈信息
	sugaredLogger.Fatalf("i am fatal, using %s", "sugar")    // FATAL 级别日志，这个会打印，附带堆栈信息，并调用 os.Exit 退出
	fmt.Println("can i be printed?")  // 这行不会打印，呃...上面已经退出了
}

```

```go
1.60837067575392e+09    info    i am info, using sugar
1.6083706757539682e+09  warn    i am warn, using sugar
1.608370675753972e+09   error   i am error, using sugar
1.6083706757539752e+09  fatal   i am fatal, using sugar

```

### 修改日期显示格式

```go
package main

import (
	"fmt"
	"go.uber.org/zap"
	"go.uber.org/zap/zapcore"
	"os"
)

func main() {
	// 配置 sugaredLogger
	var sugaredLogger *zap.SugaredLogger
	writer := zapcore.AddSync(os.Stdout)

	// 格式相关的配置
	encoderConfig := zap.NewProductionEncoderConfig()
	encoderConfig.EncodeTime = zapcore.ISO8601TimeEncoder  // 修改时间戳的格式
	encoderConfig.EncodeLevel = zapcore.CapitalLevelEncoder // 日志级别使用大写显示

	encoder := zapcore.NewConsoleEncoder(encoderConfig)

	core := zapcore.NewCore(encoder, writer, zapcore.InfoLevel)
	logger := zap.New(core)
	sugaredLogger = logger.Sugar()

	// 打印日志
	sugaredLogger.Debugf("i am debug, using %s", "sugar")   // 这行不会打印，因为日志级别是 INFO
	sugaredLogger.Infof("i am info, using %s", "sugar")      // INFO  级别日志，这个会正常打印
	sugaredLogger.Warnf("i am warn, using %s", "sugar")    // WARN  级别日志，这个会正常打印
	sugaredLogger.Errorf("i am error, using %s", "sugar")   // ERROR 级别日志，这个会打印，并附带堆栈信息
	sugaredLogger.Fatalf("i am fatal, using %s", "sugar")    // FATAL 级别日志，这个会打印，附带堆栈信息，并调用 os.Exit 退出
	fmt.Println("can i be printed?")  // 这行不会打印，呃...上面已经退出了
}

```

```go
2020-12-19T17:44:19.031+0800    INFO    i am info, using sugar
2020-12-19T17:44:19.031+0800    WARN    i am warn, using sugar
2020-12-19T17:44:19.031+0800    ERROR   i am error, using sugar
2020-12-19T17:44:19.031+0800    FATAL   i am fatal, using sugar

```

呃…看起来正常点了，上述栗子顺便把日志级别显示改成大写了，看起来更习惯一点~

### 增加 caller 信息

```go
package main

import (
	"fmt"
	"go.uber.org/zap"
	"go.uber.org/zap/zapcore"
	"os"
)

func main() {

	// 配置 sugaredLogger
	var sugaredLogger *zap.SugaredLogger
	writer := zapcore.AddSync(os.Stdout)

	// 格式相关的配置
	encoderConfig := zap.NewProductionEncoderConfig()
	encoderConfig.EncodeTime = zapcore.ISO8601TimeEncoder  // 修改时间戳的格式
	encoderConfig.EncodeLevel = zapcore.CapitalLevelEncoder // 日志级别使用大写显示
	encoder := zapcore.NewConsoleEncoder(encoderConfig)

	core := zapcore.NewCore(encoder, writer, zapcore.InfoLevel)
	logger := zap.New(core, zap.AddCaller())  // 增加 caller 信息
	sugaredLogger = logger.Sugar()

	// 打印日志
	sugaredLogger.Debugf("i am debug, using %s", "sugar")   // 这行不会打印，因为日志级别是 INFO
	sugaredLogger.Infof("i am info, using %s", "sugar")      // INFO  级别日志，这个会正常打印
	sugaredLogger.Warnf("i am warn, using %s", "sugar")    // WARN  级别日志，这个会正常打印
	sugaredLogger.Errorf("i am error, using %s", "sugar")   // ERROR 级别日志，这个会打印，并附带堆栈信息
	sugaredLogger.Fatalf("i am fatal, using %s", "sugar")    // FATAL 级别日志，这个会打印，附带堆栈信息，并调用 os.Exit 退出
	fmt.Println("can i be printed?")  // 这行不会打印，呃...上面已经退出了
}

```

```go
2020-12-19T17:50:24.257+0800    INFO    zap3/main.go:29 i am info, using sugar
2020-12-19T17:50:24.257+0800    WARN    zap3/main.go:30 i am warn, using sugar
2020-12-19T17:50:24.257+0800    ERROR   zap3/main.go:31 i am error, using sugar
2020-12-19T17:50:24.257+0800    FATAL   zap3/main.go:32 i am fatal, using sugar

```

### 修改日志级别

默认的级别的 `INFO`，我们把它改成 `DEBUG`， 这个也简单

```go
package main

import (
	"fmt"
	"go.uber.org/zap"
	"go.uber.org/zap/zapcore"
	"os"
)

func main() {

	// 配置 sugaredLogger
	var sugaredLogger *zap.SugaredLogger
	writer := zapcore.AddSync(os.Stdout)

	// 格式相关的配置
	encoderConfig := zap.NewProductionEncoderConfig()
	encoderConfig.EncodeTime = zapcore.ISO8601TimeEncoder  // 修改时间戳的格式
	encoderConfig.EncodeLevel = zapcore.CapitalLevelEncoder // 日志级别使用大写显示
	encoder := zapcore.NewConsoleEncoder(encoderConfig)

	core := zapcore.NewCore(encoder, writer, zapcore.DebugLevel)  // 将日志级别设置为 DEBUG
	logger := zap.New(core, zap.AddCaller())  // 增加 caller 信息
	sugaredLogger = logger.Sugar()

	// 打印日志
	sugaredLogger.Debugf("i am debug, using %s", "sugar")   // 这行现在可以打印出来了！
	sugaredLogger.Infof("i am info, using %s", "sugar")      // INFO  级别日志，这个会正常打印
	sugaredLogger.Warnf("i am warn, using %s", "sugar")    // WARN  级别日志，这个会正常打印
	sugaredLogger.Errorf("i am error, using %s", "sugar")   // ERROR 级别日志，这个会打印，并附带堆栈信息
	sugaredLogger.Fatalf("i am fatal, using %s", "sugar")    // FATAL 级别日志，这个会打印，附带堆栈信息，并调用 os.Exit 退出
	fmt.Println("can i be printed?")  // 这行不会打印，呃...上面已经退出了
}

```

```go
2020-12-19T17:52:29.438+0800    DEBUG   zap3/main.go:28 i am debug, using sugar
2020-12-19T17:52:29.439+0800    INFO    zap3/main.go:29 i am info, using sugar
2020-12-19T17:52:29.439+0800    WARN    zap3/main.go:30 i am warn, using sugar
2020-12-19T17:52:29.439+0800    ERROR   zap3/main.go:31 i am error, using sugar
2020-12-19T17:52:29.439+0800    FATAL   zap3/main.go:32 i am fatal, using sugar

```

可以看到 `DEBUG` 级别的日志也打印出来了。

### 添加自定义字段

有时候我们需要在每行日志中都加入一些字段，用来标识这些日志，可以在调用 `zap.New()` 创建 `logger` 的时候把这些字段设置上去，如下

```go
package main

import (
	"fmt"
	"github.com/google/uuid"
	"go.uber.org/zap"
	"go.uber.org/zap/zapcore"
	"os"
)

func main() {

	// 配置 sugaredLogger
	var sugaredLogger *zap.SugaredLogger
	writer := zapcore.AddSync(os.Stdout)

	// 格式相关的配置
	encoderConfig := zap.NewProductionEncoderConfig()
	encoderConfig.EncodeTime = zapcore.ISO8601TimeEncoder  // 修改时间戳的格式
	encoderConfig.EncodeLevel = zapcore.CapitalLevelEncoder // 日志级别使用大写显示
	encoder := zapcore.NewConsoleEncoder(encoderConfig)

	core := zapcore.NewCore(encoder, writer, zapcore.DebugLevel)  // 将日志级别设置为 DEBUG
	logger := zap.New(core, zap.AddCaller(), zap.Fields(zapcore.Field{  // 添加 uuid 字段
		Key:       "uuid",
		Type:      zapcore.StringType,
		String:    uuid.New().String(),
	}))
	sugaredLogger = logger.Sugar()

	// 打印日志
	sugaredLogger.Debugf("i am debug, using %s", "sugar")   // 这行现在可以打印出来了！
	sugaredLogger.Infof("i am info, using %s", "sugar")      // INFO  级别日志，这个会正常打印
	sugaredLogger.Warnf("i am warn, using %s", "sugar")    // WARN  级别日志，这个会正常打印
	sugaredLogger.Errorf("i am error, using %s", "sugar")   // ERROR 级别日志，这个会打印，并附带堆栈信息
	sugaredLogger.Fatalf("i am fatal, using %s", "sugar")    // FATAL 级别日志，这个会打印，附带堆栈信息，并调用 os.Exit 退出
	fmt.Println("can i be printed?")  // 这行不会打印，呃...上面已经退出了
}

```

```go
2020-12-19T18:02:15.093+0800    DEBUG   zap3/main.go:32 i am debug, using sugar {"uuid": "79432609-3ae9-4728-bbd2-f368d404018d"}
2020-12-19T18:02:15.093+0800    INFO    zap3/main.go:33 i am info, using sugar  {"uuid": "79432609-3ae9-4728-bbd2-f368d404018d"}
2020-12-19T18:02:15.093+0800    WARN    zap3/main.go:34 i am warn, using sugar  {"uuid": "79432609-3ae9-4728-bbd2-f368d404018d"}
2020-12-19T18:02:15.093+0800    ERROR   zap3/main.go:35 i am error, using sugar {"uuid": "79432609-3ae9-4728-bbd2-f368d404018d"}
2020-12-19T18:02:15.093+0800    FATAL   zap3/main.go:36 i am fatal, using sugar {"uuid": "79432609-3ae9-4728-bbd2-f368d404018d"}

```

