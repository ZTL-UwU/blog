# powerlevel10k: 最好看，方便，实用的命令行主题

[项目地址](https://github.com/romkatv/powerlevel10k)

## 安装

### Linux

1. 安装 Zsh

    ```zsh
    sudo apt install zsh
    ```

2. 安装 Oh-My-Zsh

    | 方法  |                                               命令                                                |
    | :---: | :-----------------------------------------------------------------------------------------------: |
    | curl  | `sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"` |
    | wget  |  `sh -c "$(wget -O- https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"`  |

3. 安装 P10k

    ```zsh
    git clone --depth=1 https://github.com/romkatv/powerlevel10k.git ${ZSH_CUSTOM:-$HOME/.oh-my-zsh/custom}/themes/powerlevel10k
    ```

    Gitee 镜像

    ```zsh
    git clone --depth=1 https://gitee.com/romkatv/powerlevel10k.git ${ZSH_CUSTOM:-$HOME/.oh-my-zsh/custom}/themes/powerlevel10k
    ```

    在　`~/.zshrc` 中设置 `ZSH_THEME`

    ```zsh
    ZSH_THEME="powerlevel10k/powerlevel10k"
    ```

4. 重启命令行

5. 设置 P10k

    ```zsh
    p10k configure
    ```

    ![configure](https://s1.ax1x.com/2020/10/07/0dWc1s.gif)

然后... 完成! 是不是很简单?

### Windows

~~什么? 你居然在用 Windows? 洗洗睡吧。~~

什么? 你居然在用 Win10, 好像有希望!

1. 打开你的 WSL (我死了)

2. 和 Linux 一毛一样

    ~~既然都用 WSL 了，怎么不装个 Linux 呢?~~

### 字体问题

![字体爆炸](https://s1.ax1x.com/2020/10/07/0d2t4H.png)

⇡⇡⇡ ~~诶, 你怎么说装好了呢, 明明就不行, 太菜了!~~ ⇡⇡⇡

这个嘛，因为里面有这个
![尖头](https://s1.ax1x.com/2020/10/07/0d2R8s.png)
, 所以需要安装专门的控制台字体。

个人推荐 `Fira Code` 和 `MesloLGS NF` 或可以使用这个仓库 [https://github.com/powerline/fonts](https://github.com/powerline/fonts) 中的任意字体。

1. Gnome-terminal

    在 Gnome-terminal(Ubuntu 默认终端) 中在 Preference(首选项)中可以修改。⇣ 如下图所示。⇣

    ![Gnome-terminal](https://s1.ax1x.com/2020/10/07/0dReqf.png)

2. VScode

    在 VScode 中可以按下 `Ctrl + ,` 直接在 Settings(设置)中修改。⇣ 如下图所示。⇣

    ![VScode-terminal](https://s1.ax1x.com/2020/10/07/0dRUdU.png)

~~这个有人不会吗?~~

### 配置

配置文件存在 `~/.p10k.zsh` 中，打开可以更改内部设置。具体方式这里不过多叙述，需要了解可以查看 [原仓库 Readme](https://github.com/romkatv/powerlevel10k/blob/master/README.md)

#### 这是左边区域的配置

```zsh
32 |  # The list of segments shown on the left. Fill it with the most important segments.
33 |  typeset -g POWERLEVEL9K_LEFT_PROMPT_ELEMENTS=(
34 |    # =========================[ Line #1 ]=========================
35 |    # os_icon               # os identifier
36 |    dir                     # current directory
37 |    vcs                     # git status
38 |    # =========================[ Line #2 ]=========================
39 |    newline                 # \n
40 |    # prompt_char           # prompt symbol
41 |  )
```

#### 这是右边区域的配置

```zsh
47  |  typeset -g POWERLEVEL9K_RIGHT_PROMPT_ELEMENTS=(
48  |    # =========================[ Line #1 ]=========================
49  |    status                  # exit code of the last command
50  |    command_execution_time  # duration of the last command
51  |    background_jobs         # presence of background jobs
52  |    direnv                  # direnv status (https://direnv.net/)
53  |    asdf                    # asdf version manager (https://github.com/asdf-vm/asdf)
54  |    virtualenv              # python virtual environment (https://docs.python.org/3/library/venv.html)
55  |    anaconda                # conda environment (https://conda.io/)
.   |   .
.   |   .
.   |   .
101 |    todo                    # todo items (https://github.com/todotxt/todo.txt-cli)
102 |    # swap                  # used swap
103 |    time                    # current time
104 |    # =========================[ Line #2 ]=========================
105 |    newline
106 |    # ip                    # ip address and bandwidth usage for a specified network interface
107 |    # public_ip             # public IP address
108 |    # proxy                 # system-wide http/https/ftp proxy
109 |    # battery               # internal battery
110 |    # wifi                  # wifi speed
111 |    # example               # example user-defined segment (see prompt_example function below)
112 |  )
```

取消主矢或添加注释可以增添显示的功能方块

[各个功能的作用](https://github.com/romkatv/powerlevel10k/blob/master/README.md#batteries-included)

![所有功能都打开的沙雕样子](https://s3.ax1x.com/2020/12/11/rE1zh8.png)

这些功能中比较有用的，比如:

|         英文名         |            作用            |
| :--------------------: | :------------------------: |
|         status         | 上次运行的指令的运行返回值 |
| command_execution_time |   上次指令运行经历的时间   |
|          time          |          显示时间          |
|          load          |          CPU 负载          |
|          ram           |        显示剩余内存        |

通过修改

```zsh
typeset -g POWERLEVEL9K_***_VISUAL_IDENTIFIER_EXPANSION='⭐'
                                                         ^~~ 这里
```

可以改变某功能的图标
