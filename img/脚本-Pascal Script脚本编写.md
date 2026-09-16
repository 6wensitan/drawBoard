rename

> 脚本学习

# Pascal Script rule

This rule allows you define your own renaming logic and integrate external tools. It uses the [Pascal Script programming engine](https://www.den4b.com/docs/books/renamer/page/pascal-script-overview) with syntax and conventions similar to Delphi/Pascal language. The rule comes with a number of preloaded example scripts. We will see how to use them, and how to write your own script.

[![ReNamer Pascal Script rule.png](https://www.den4b.com/docs/uploads/images/gallery/2026-01/scaled-1680-/renamer-pascal-script-rule.png)](https://www.den4b.com/docs/uploads/images/gallery/2026-01/renamer-pascal-script-rule.png)

### Writing your own script

To write your own script, you need to have knowledge of Pascal Script syntax and conventions, and be familiar with the list of built-in types and functions. All that you can learn using a dedicated [Pascal Script reference](https://www.den4b.com/docs/books/renamer/chapter/pascal-script) section.

Let's assume that you already know how to write scripts.

The general workflow is as follows: 

1. Write your script, or load one from an external source.
   - You can also use the built-in examples via the *Scripts* menu, or take a script from the [Examples](https://www.den4b.com/docs/books/renamer/page/pascal-script-examples) and the [Library](https://www.den4b.com/docs/books/renamer/page/pascal-script-library) reference sections. More on that in the section below.
2. Compile the script by pressing the *Try to Compile* button.
   - If an error message comes up, troubleshoot the script. The error message usually includes the line number of the problematic statement in the script. Press the *Go To* button and enter that line number to locate the faulty statement quickly. Once you correct the fault, press the *Try to Compile* button again. Repeat until you see a *Compiled successfully!* message.
3. Add the rule to your renaming stack by clicking the *Add Rule* button.
   - Optionally, you can save the script to the built-in repository via the *Scripts* menu, for quick access and reuse later.

The editor provides basic syntax highlighting and auto-completion for built-in functions, via the `CTRL+Space` shortcut.

To get you started, let's try this example script, which will simply prefix each filename with "Example" text:

```
begin

  FileName := 'Example ' + FileName;

end.
```





### Loading a script

You can access your local scripts repository via the the *Scripts* menu. The menu will show you a list of preloaded example scripts and any scripts which you have saved previously. Click on any of the script names in the menu to load it into the editor.

You can assemble your script using multiple scripts by holding down the `SHIFT` key when loading a script, which will insert it inline instead of replacing the whole script, just make sure to combine them correctly.

In addition to the preloaded example scripts, you can find more examples and user contributed scripts at:

- [Examples repository](https://www.den4b.com/docs/books/renamer/page/pascal-script-examples) for simple how-to examples.
- [Library repository](https://www.den4b.com/docs/books/renamer/page/pascal-script-library) for advanced examples and 3rd party tool integrations.
- [User Forum](https://www.den4b.com/forum/) for user contributions.







### 实际需要编码

源文件：

![image-20260820142321854](C:\Users\LOPIE\AppData\Roaming\Typora\typora-user-images\image-20260820142321854.png)

补充规则：

![image-20260820160008293](C:\Users\LOPIE\AppData\Roaming\Typora\typora-user-images\image-20260820160008293.png)



要求：

目标文件名格式：`GZJCK_`截取后目标`_提单`

> C:\Users\LOPIE\Documents\den4b\ReNamer\Presets\船务文件匹配规则.rnp

![image-20260820161529272](C:\Users\LOPIE\AppData\Roaming\Typora\typora-user-images\image-20260820161529272.png)

![image-20260820145840762](C:\Users\LOPIE\AppData\Roaming\Typora\typora-user-images\image-20260820145840762.png)





