# [Getting Started](@id man-getting-started)

Julia installation is straightforward, whether using precompiled binaries or compiling from source. Download and install Julia by following the instructions at <https://julialang.org/downloads/>.

The easiest way to learn and experiment with Julia is by starting an interactive session (also known as a read-eval-print loop or "REPL") by double-clicking the Julia executable or running `julia` from the command line:

```@eval
io = IOBuffer()
Base.banner(io)
banner = String(take!(io))
import Markdown
Markdown.parse("```\n\$ julia\n\n$(banner)\njulia> 1 + 2\n3\n\njulia> ans\n3\n```")
```

To exit the interactive session, type `CTRL-D` (press the Control/`^` key together with the `d` key), or type `exit()`. When run in interactive mode, `julia` displays a banner and prompts the user for input. Once the user has entered a complete expression, such as `1 + 2`, and hits enter, the interactive session evaluates the expression and shows its value. If an expression is entered into an interactive session with a trailing semicolon, its value is not shown. The variable `ans` is bound to the value of the last evaluated expression whether it is shown or not. The `ans` variable is only bound in interactive sessions, not when Julia code is run in other ways.

To evaluate expressions written in a source file `file.jl`, write `include("file.jl")`.

To run code in a file non-interactively, you can give it as the first argument to the `julia` command:

    $ julia script.jl arg1 arg2...
    

As the example implies, the following command-line arguments to `julia` are interpreted as command-line arguments to the program `script.jl`, passed in the global constant `ARGS`. The name of the script itself is passed in as the global `PROGRAM_FILE`. Note that `ARGS` is also set when a Julia expression is given using the `-e` option on the command line (see the `julia` help output below) but `PROGRAM_FILE` will be empty. For example, to just print the arguments given to a script, you could do this:

    $ julia -e 'println(PROGRAM_FILE); for x in ARGS; println(x); end' foo bar
    
    foo
    bar
    

Or you could put that code into a script and run it:

    $ echo 'println(PROGRAM_FILE); for x in ARGS; println(x); end' > script.jl
    $ julia script.jl foo bar
    script.jl
    foo
    bar
    

The `--` delimiter can be used to separate command-line arguments intended for the script file from arguments intended for Julia:

    $ julia --color=yes -O -- foo.jl arg1 arg2..
    

Julia can be started in parallel mode with either the `-p` or the `--machine-file` options. `-p n` will launch an additional `n` worker processes, while `--machine-file file` will launch a worker for each line in file `file`. The machines defined in `file` must be accessible via a password-less `ssh` login, with Julia installed at the same location as the current host. Each machine definition takes the form `[count*][user@]host[:port] [bind_addr[:port]]`. `user` defaults to current user, `port` to the standard ssh port. `count` is the number of workers to spawn on the node, and defaults to 1. The optional `bind-to bind_addr[:port]` specifies the IP address and port that other workers should use to connect to this worker.

If you have code that you want executed whenever Julia is run, you can put it in `~/.julia/config/startup.jl`:

    $ echo 'println("Greetings! 你好! 안녕하세요?")' > ~/.julia/config/startup.jl
    $ julia
    Greetings! 你好! 안녕하세요?
    
    ...
    

There are various ways to run Julia code and provide options, similar to those available for the `perl` and `ruby` programs:

    julia [switches] -- [programfile] [args...]Set 

<dir>
  as the home project/environment. The default @. option will search through parent directories until a Project.toml or JuliaProject.toml file is found.</td> </tr> 
  
  <tr>
    <td align="left">
      <code>-J</code>, <code>--sysimage &lt;file&gt;</code>
    </td>
    
    <td align="left">
      Start up with the given system image file
    </td>
  </tr>
  
  <tr>
    <td align="left">
      <code>-H</code>, <code>--home &lt;dir&gt;</code>
    </td>
    
    <td align="left">
      Set location of <code>julia</code> executable
    </td>
  </tr>
  
  <tr>
    <td align="left">
      <code>--startup-file={yes\|no}</code>
    </td>
    
    <td align="left">
      Load <code>~/.julia/config/startup.jl</code>
    </td>
  </tr>
  
  <tr>
    <td align="left">
      <code>--handle-signals={yes\|no}</code>
    </td>
    
    <td align="left">
      Enable or disable Julia's default signal handlers
    </td>
  </tr>
  
  <tr>
    <td align="left">
      <code>--sysimage-native-code={yes\|no}</code>
    </td>
    
    <td align="left">
      Use native code from system image if available
    </td>
  </tr>
  
  <tr>
    <td align="left">
      <code>--compiled-modules={yes\|no}</code>
    </td>
    
    <td align="left">
      Enable or disable incremental precompilation of modules
    </td>
  </tr>
  
  <tr>
    <td align="left">
      <code>-e</code>, <code>--eval &lt;expr&gt;</code>
    </td>
    
    <td align="left">
      Evaluate <code>&lt;expr&gt;</code>
    </td>
  </tr>
  
  <tr>
    <td align="left">
      <code>-E</code>, <code>--print &lt;expr&gt;</code>
    </td>
    
    <td align="left">
      Evaluate <code>&lt;expr&gt;</code> and display the result
    </td>
  </tr>
  
  <tr>
    <td align="left">
      <code>-L</code>, <code>--load &lt;file&gt;</code>
    </td>
    
    <td align="left">
      Load <code>&lt;file&gt;</code> immediately on all processors
    </td>
  </tr>
  
  <tr>
    <td align="left">
      <code>-p</code>, <code>--procs {N\|auto</code>}
    </td>
    
    <td align="left">
      Integer value N launches N additional local worker processes; <code>auto</code> launches as many workers as the number of local CPU threads (logical cores)
    </td>
  </tr>
  
  <tr>
    <td align="left">
      <code>--machine-file &lt;file&gt;</code>
    </td>
    
    <td align="left">
      Run processes on hosts listed in <code>&lt;file&gt;</code>
    </td>
  </tr>
  
  <tr>
    <td align="left">
      <code>-i</code>
    </td>
    
    <td align="left">
      Interactive mode; REPL runs and <code>isinteractive()</code> is true
    </td>
  </tr>
  
  <tr>
    <td align="left">
      <code>-q</code>, <code>--quiet</code>
    </td>
    
    <td align="left">
      Quiet startup: no banner, suppress REPL warnings
    </td>
  </tr>
  
  <tr>
    <td align="left">
      <code>--banner={yes\|no\|auto}</code>
    </td>
    
    <td align="left">
      Enable or disable startup banner
    </td>
  </tr>
  
  <tr>
    <td align="left">
      <code>--color={yes\|no\|auto}</code>
    </td>
    
    <td align="left">
      Enable or disable color text
    </td>
  </tr>
  
  <tr>
    <td align="left">
      <code>--history-file={yes\|no}</code>
    </td>
    
    <td align="left">
      Load or save history
    </td>
  </tr>
  
  <tr>
    <td align="left">
      <code>--depwarn={yes\|no\|error}</code>
    </td>
    
    <td align="left">
      Enable or disable syntax and method deprecation warnings (<code>error</code> turns warnings into errors)
    </td>
  </tr>
  
  <tr>
    <td align="left">
      <code>--warn-overwrite={yes\|no}</code>
    </td>
    
    <td align="left">
      Enable or disable method overwrite warnings
    </td>
  </tr>
  
  <tr>
    <td align="left">
      <code>-C</code>, <code>--cpu-target &lt;target&gt;</code>
    </td>
    
    <td align="left">
      Limit usage of CPU features up to <code>&lt;target&gt;</code>; set to <code>help</code> to see the available options
    </td>
  </tr>
  
  <tr>
    <td align="left">
      <code>-O</code>, <code>--optimize={0,1,2,3}</code>
    </td>
    
    <td align="left">
      Set the optimization level (default level is 2 if unspecified or 3 if used without a level)
    </td>
  </tr>
  
  <tr>
    <td align="left">
      <code>-g</code>, <code>-g &lt;level&gt;</code>
    </td>
    
    <td align="left">
      Enable / Set the level of debug info generation (default level is 1 if unspecified or 2 if used without a level)
    </td>
  </tr>
  
  <tr>
    <td align="left">
      <code>--inline={yes\|no}</code>
    </td>
    
    <td align="left">
      Control whether inlining is permitted, including overriding <code>@inline</code> declarations
    </td>
  </tr>
  
  <tr>
    <td align="left">
      <code>--check-bounds={yes\|no}</code>
    </td>
    
    <td align="left">
      Emit bounds checks always or never (ignoring declarations)
    </td>
  </tr>
  
  <tr>
    <td align="left">
      <code>--math-mode={ieee,fast}</code>
    </td>
    
    <td align="left">
      Disallow or enable unsafe floating point optimizations (overrides @fastmath declaration)
    </td>
  </tr>
  
  <tr>
    <td align="left">
      <code>--code-coverage={none\|user\|all}</code>
    </td>
    
    <td align="left">
      Count executions of source lines
    </td>
  </tr>
  
  <tr>
    <td align="left">
      <code>--code-coverage</code>
    </td>
    
    <td align="left">
      equivalent to <code>--code-coverage=user</code>
    </td>
  </tr>
  
  <tr>
    <td align="left">
      <code>--track-allocation={none\|user\|all}</code>
    </td>
    
    <td align="left">
      Count bytes allocated by each source line
    </td>
  </tr>
  
  <tr>
    <td align="left">
      <code>--track-allocation</code>
    </td>
    
    <td align="left">
      equivalent to <code>--track-allocation=user</code>
    </td>
  </tr></tbody> </table> 
  
  <h2>
    Resources
  </h2>
  
  <p>
    A curated list of useful learning resources to help new users get started can be found on the <a href="https://julialang.org/learning/">learning</a> page of the main Julia web site.
  </p>