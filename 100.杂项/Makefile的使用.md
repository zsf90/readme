## Makefile 的规则

在讲述这个makefile之前，还是让我们先来粗略地看一看makefile的规则。

```makefile
target ... : prerequisites ...
	command
	...
	...
```

### target

可以是一个 object file（目标文件），也可以是一个执行文件，还可以是一个标签（label）。

### prerequisites

生成该 target 所依赖的文件和/或target

### command

该 target 要执行的命令（任意的shell命令）

这是一个文件的依赖关系，也就是说，target 这一个或多个的目标文件依赖于 prerequisites 中的文件，其生成规则定义在 command 中。说白一点就是说：

```
prerequisites 中如果有一个以上的文件比 target 文件要新的话，command 所定义的命令就会被执行。
```

这就是 makefile 的规则，也就是 makefile 中最核心的内容。

