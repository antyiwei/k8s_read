# command 结构


 ```go
    // Command 是一个命令，适用于您的应用程序。例如。 'go run ...' - 'run'是命令。 Cobra要求您将用法和描述定义为命令定义的一部分，以确保可用性。
    type Command struct {
        // 使用是一行使用消息。.
        Use string
    
        // 别名是一组别名，可用于代替使用中的第一个单词.
        Aliases []string
    
        // SuggestFor是一个命令名数组，建议使用此命令 - 类似于别名但仅建议。
        SuggestFor []string
    
        // Short是“帮助”输出中显示的简短描述.
        Short string
    
        // Long是'help <this-command>'输出中显示的长消息.
        Long string
    
        // 是如何使用该命令的示例.
        Example string
    
        // ValidArgs是bash完成中接受的所有有效非标志参数的列表
        ValidArgs []string
    
        // Expected arguments
        Args PositionalArgs
    
        // ArgAliases是ValidArgs的别名列表。在bash完成时不向用户建议，但如果手动输入则接受.
        ArgAliases []string
    
        // BashCompletionFunction 是bash自动完成生成器使用的自定义函数。
        BashCompletionFunction string
    
        // 如果不推荐使用此命令，则不推荐使用定义，并且在使用时应打印此字符串.
        Deprecated string
    
        // 隐藏定义，如果此命令被隐藏，并且不应显示在可用命令列表中.
        Hidden bool
    
        // 注释是应用程序可用于标识或分组命令的键/值对.
        Annotations map[string]string
    
        // 版本定义此命令的版本。如果此值为非空且命令未定义“version”标志，则将在命令中添加“version”布尔标志，如果指定，将打印“Version”变量的内容.
        Version string
    
        //   * 运行功能按以下顺序执行:
        //   * PersistentPreRun()
        //   * PreRun()
        //   * Run()
        //   * PostRun()
        //   * PersistentPostRun()
        // 所有函数都获得相同的args，即命令名后面的参数.
        //
        // PersistentPreRun：此命令的子节点将继承并执行.
        PersistentPreRun func(cmd *Command, args []string)
        // PersistentPreRunE：PersistentPreRun但返回错误.
        PersistentPreRunE func(cmd *Command, args []string) error
        // PreRun：此命令的子节点不会继承。
        PreRun func(cmd *Command, args []string)
        // PreRunE：PreRun但返回错误。
        PreRunE func(cmd *Command, args []string) error
        // Run: 通常是实际的工作功能。大多数命令只会实现这一点.
        Run func(cmd *Command, args []string)
        // RunE：运行但返回错误。
        RunE func(cmd *Command, args []string) error
        // PostRun：运行Run命令后运行。
        PostRun func(cmd *Command, args []string)
        // PostRunE：PostRun但返回错误。
        PostRunE func(cmd *Command, args []string) error
        // PersistentPostRun：此命令的子节点将在PostRun之后继承并执行。
        PersistentPostRun func(cmd *Command, args []string)
        // PersistentPostRunE：PersistentPostRun但返回错误。
        PersistentPostRunE func(cmd *Command, args []string) error
    
        // SilenceErrors是下游安静错误的一种选择。
        SilenceErrors bool
    
        // SilenceUsage是在发生错误时静默使用的选项。
        SilenceUsage bool
    
        // DisableFlagParsing禁用标志解析​​。如果这是真的，所有标志将作为参数传递给命令。
        DisableFlagParsing bool
    
        // DisableAutoGenTag定义，如果生成gen标签（“由spf13 / cobra生成的自动...”）将通过生成此命令的文档来打印。
        DisableAutoGenTag bool
    
        // 在打印帮助或生成文档时，DisableFlagsInUseLine将禁用在命令的使用行中添加[flags]
        DisableFlagsInUseLine bool
    
        // DisableSuggestions禁用基于Levenshtein距离的建议以及“未知命令”消息。
        DisableSuggestions bool
        // SuggestionsMinimumDistance定义显示建议的最小levenshtein距离。必须> 0。
        SuggestionsMinimumDistance int
    
        // TraverseChildren在执行子命令之前解析所有父项的标志。
        TraverseChildren bool
    
        // commands 是此程序支持的命令列表。
        commands []*Command
        // parent是此命令的父命令。
        parent *Command
        // Max lengths of commands' string lengths for use in padding.
        // 译：用于填充的命令字符串长度的最大长度。
        commandsMaxUseLen         int
        commandsMaxCommandPathLen int
        commandsMaxNameLen        int
        // 如果命令切片是否已排序，则commandsAreSorted定义。
        commandsAreSorted bool
        // commandCalledAs是用于调用此命令的名称或别名值。
        commandCalledAs struct {
            name   string
            called bool
        }
    
        // args是从flags解析的实际args。
        args []string
        // flagErrorBuf包含来自pflag的所有错误消息。
        flagErrorBuf *bytes.Buffer
        // flags是一整套标志。
        flags *flag.FlagSet
        // pflags包含持久标志。
        pflags *flag.FlagSet
        // lflags包含本地标志
        lflags *flag.FlagSet
        // iflags contains inherited flags.
        iflags *flag.FlagSet
        // parentsPflags是cmd父母的所有持久标志。
        parentsPflags *flag.FlagSet
        // globNormFunc是我们可以在每个pflag set和children命令上使用的全局规范化函数
        globNormFunc func(f *flag.FlagSet, name string) flag.NormalizedName
    
        // output是用户定义的输出writer.
        output io.Writer
        // usageFunc是user定义的用法func.
        usageFunc func(*Command) error
        // usageTemplate是用户定义的用法模板。
        usageTemplate string
        // flagErrorFunc是用户定义的func，当标志解析返回错误时调用它。
        flagErrorFunc func(*Command, error) error
        // helpTemplate是用户定义的帮助模板.
        helpTemplate string
        // helpFunc是用户定义的help func.
        helpFunc func(*Command, []string)
        // helpCommand是使用'help'的命令。如果它不是由用户定义的，则cobra使用默认帮助命令。
        helpCommand *Command
        // versionTemplate是用户定义的版本模板。
        versionTemplate string
    }
```