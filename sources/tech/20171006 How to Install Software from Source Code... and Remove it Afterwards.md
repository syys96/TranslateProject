"Translating by syys96"

如何从源代码安装软件并删除源代码
============================================================

![How to install software from source code](https://itsfoss.com/wp-content/uploads/2017/10/install-software-from-source-code-linux-800x450.jpg)


 摘要：这篇教程详细地介绍了在linux上如何从源代码安装程序以及如何删除源代码的方法。

你所拥有的linux发行版的最大优点之一就是它的包管理工具和相应的软件仓库。通过它们,你可以下载所有必需的工具和资源，然后以完全自动化的方式把新的软件安装到你的电脑上。

但是，无论这些包维护者如何努力，也不可能能够解决所有用户遇到的每一个问题。他们也不能够将所有的软件都打包并放在仓库中。所以有时你仍然需要自己编译、安装新的软件。对于我来说，到目前为止自己编译软件最常见的原因是我需要某个特定的版本，或者是我想要修改源代码或者使用一些特别的编译选项。

如果你的需求属于后一种，那么有可能你已经知道了该如何去做。但是对于绝大部分的linux用户来说，第一次从源代码编译、安装一个软件就像入会仪式一样：会有一点害怕，但如果你克服了它你就可以进入一个新的世界并成为一个特权社区的一员。

[推荐阅读：如何在UbuntuU中安装和卸载软件 [完整教程]][8]


### A. 在linux中从源代码安装软件

这正是我们接下来将要做的。为了完成我们的目标，我们需要在系统中安装 [NodeJS][9] 8.1.1这个特定的版本。这个版本在Debian仓库中是没有的:
```
sh$ apt-cache madison nodejs | grep amd64
    nodejs | 6.11.1~dfsg-1 | http://deb.debian.org/debian experimental/main amd64 Packages
    nodejs | 4.8.2~dfsg-1 | http://ftp.fr.debian.org/debian stretch/main amd64 Packages
    nodejs | 4.8.2~dfsg-1~bpo8+1 | http://ftp.fr.debian.org/debian jessie-backports/main amd64 Packages
    nodejs | 0.10.29~dfsg-2 | http://ftp.fr.debian.org/debian jessie/main amd64 Packages
    nodejs | 0.10.29~dfsg-1~bpo70+1 | http://ftp.fr.debian.org/debian wheezy-backports/main amd64 Packages
```

### 第一步: 从GitHub获得源代码

和许多其它开源项目一样,NodeJS的源代码可以在GitHub上找到:: [https://github.com/nodejs/node][10]
因此我们直接打开这个页面.

![The NodeJS official GitHub repository](https://itsfoss.com/wp-content/uploads/2017/07/nodejs-github-account.png)

如果你并不熟悉GitHub、git或者其它版本控制系统，那么需要说明的是仓库包含了该软件目前的的源代码以及历年对这个软件的所有修改的记录,并最后出现在这个项目的第一行。对于开发者来说，保存这些修改历史有许多好处。对于现在的我们来说，主要的好处就是我们可以得到这个项目在任一时期的代码。更为准确地说，尽管在8.1.1版本之后代码又做了许多修改，我仍然可以获得这个项目在8.1.1版本时发布的代码。

![Choose the v8.1.1 tag in the NodeJS GitHub repository](https://itsfoss.com/wp-content/uploads/2017/07/nodejs-github-choose-revision-tag.png)

在GitHub上,你可以通过“分支”按钮在软件的不同版本之间切换浏览 [“Branch” and “tags” are somewhat related concepts in Git][14].开发者最基本的操作包括创建“分支”和“标记”来跟踪工程开发过程中的重要事件，比如他们他们何时开始开发一个新的功能或者何时发布一个发行版。在这里我不会深入这些细节，你只需要知道我正在寻找标记为“v8.1.1”的版本。

![The NodeJS GitHub repository as it was at the time the v8.1.1 tag was created](https://itsfoss.com/wp-content/uploads/2017/07/nodejs-github-revision-811.png)

当我们选择了"v8.1.1"标记之后，页面随之刷新。最明显的变化就是标记现在成为了URL的一部分。另外，你会注意到文件的更改日期也与之前不同。你现在看到的资源目录也就是创建“v8.1.1”这个标记时的资源目录。在某种意义上你可以把git之类的版本控制工具当作一个时间旅行机器，这个机器允许你回到过去并进入一个项目的历史之中。

![NodeJS GitHub repository download as a ZIP button](https://itsfoss.com/wp-content/uploads/2017/07/nodejs-github-revision-download-zip.png)

现在我们可以下载NodeJS8.1.1的源代码。很明显的大蓝色按钮意为下载ZIP文件。为了更好地解释过程，我会在命令行中下载并解压提取ZIP文件。但是如果你更喜欢使用  [图形用户界面][15] 工具，你也可以那样做:
```
wget https://github.com/nodejs/node/archive/v8.1.1.zip
unzip v8.1.1.zip
cd node-8.1.1/
```
直接下载ZIP文件是很好的方法，但是如果你想要更为专业地完成这个步骤的话，我建议你直接使用 `git` 来下载这些资源。这个操作并不复杂，并且这也会让你开始接触到一个经常会用到的工具。

```
# first ensure git is installed on your system
sh$ sudo apt-get install git
# Make a shallow clone the NodeJS repository at v8.1.1
sh$ git clone --depth 1 \
              --branch v8.1.1 \
              https://github.com/nodejs/node
sh$ cd node/
```

顺便一提，如果到目前为止你有任何疑问的话，那么可以把这篇文章的第一部分看作一个整体的介绍。为了帮助解决你解决常规的问题，之后我会对Debian和ReadHat发行版做更为详细的介绍。
```
sh$ ls
android-configure  BUILDING.md            common.gypi      doc            Makefile   src
AUTHORS            CHANGELOG.md           configure        GOVERNANCE.md  node.gyp   test
benchmark          CODE_OF_CONDUCT.md     CONTRIBUTING.md  lib            node.gypi  tools
BSDmakefile        COLLABORATOR_GUIDE.md  deps             LICENSE        README.md  vcbuild.bat
```

### 第二布: 理解程序的构建系统

我们通常说“编译源代码”，但是编译只是把源代码变成可以运行的软件的整个过程的一个部分。构建系统指的是一系列的工具和操作，它们使得这些不同的任务自动化和清晰化，以此用较少的命令来构建整个软件。

如果这个概念本身非常简单，那么实际操作则更为复杂。因为不同的项目或者不同的编程语言可能有许多不同的要求。或者是程序员的癖好不同，或者是支持的系统有区别，或者是历史的原因等等……几乎有数不尽的理由去选择或者创造另一个构建系统。总而言之，在这里有多种不同的方法可以供你选择。

NodeJS使用了一个 [GNU风格的构建系统][16].在开源社区中这是一个大众化的选择，并且也是开始你工作的良好途径。

编写并调试一个构建系统是一个相当复杂的工作，但是对于终端用户来说，GNU风格的构建系统为他们提供了两个有用的工具 `configure` and `make`。

配置文件是一个与项目相关的脚本，这个脚本可以检查目标系统的配置以及可用的特性，从而保证项目能够成功构建。最终这个脚本还会当前平台的有关细节。

一个典型的 `configure` 任务的重要一环是创建Makefile。Makefile包含了用于有效地构建整个项目的所有命令。

另一方面， [`make` 工具][17]是一个在类Unix系统的可移植操作系统接口工具。它可以读取项目的Makefile然后执行相应的命令来编译和安装你的程序。

但是，如同在linux世界中的那样，你依然可以为你的特殊需求制定相应的构建方法。
```
./configure --help
```

 `configure -help` 命令会显示所有可选的配置选项。当然，这也是与特定项目想关的。老实说，有时你需要深入项目内部才能完全理解每一个配置选项的含义。

但是你至少需要知道一个标准的GNU自动工具选项： `--prefix` 前缀.它与文件系统结构和你的程序将要安装的位置有关。

[推荐阅读的8个Vim技巧，让你成为专业的用户][18]

### Step 3: The FHS

一个典型的linux发行版的文件系统结构大多数都遵守 [文件系统结构标准 (FHS)][19]

这个标准阐释了你的系统的各种目录如/usr,/tmp, /var等的主要功能。

当我们使用GNU自动工具和其他大多数构建系统时，你的新软件的默认安装位置是 `/usr/local`。根据FSH“/usr/local目录是用于系统管理员在本地安装软件吗？这个目录需要在系统软件更新时确保不被覆盖掉，它可用于放置主机之间的，在/usr.目录找不到的公用程序和数据”，这个位置是一个很好的选择。

 `/usr/local` 目录某种意义上是根目录的复制版本，你可以在 `/usr/local/bin` 找到可执行文件，在 `/usr/local/lib` 找到库文件，在 `/usr/local/share` 找到独立于体系结构的文件等。

当你把 `/usr/local` 作为常规软件的安装路径时，你唯一需要注意的是你的软件会与该目录下的其它软件放在一起。尤其是当你安装了好几个软件后，很难再分清 `/usr/local/bin` 下的哪些文件和 `/usr/local/lib` 下的哪些文件属于同一个软件。当然 `/usr/bin` 也是同样的混乱，在你想要卸载一个手工安装的软件的时侯就会出现这个问题。

为了解决这个问题，我倾向于把常规软件安装到 `/opt`子目录下。在这里再一次引用FHS:

_”/opt 目录用于安装附加的应用软件包。安装于/opt位置的软件包需要能够找到其在/opt/"package"或者/opt/"provider"的静态文件。其中"package"是这个软件包的名称，“provider”是软件提供者在LANANA的注册名称。”_

所以我们需要创建一个 `opt` 的子目录用于安装NodeJS,如果用一天我想要卸载这个软件，我就可以直接删除这个目录:


```
sh$ sudo mkdir /opt/node-v8.1.1
sh$ sudo ln -sT node-v8.1.1 /opt/node
# What is the purpose of the symbolic link above?
# Read the article till the end--then try to answer that
# question in the comment section!

sh$ ./configure --prefix=/opt/node-v8.1.1
sh$ make -j9 && echo ok
# -j9 means run up to 9 parallel tasks to build the software.
# As a rule of thumb, use -j(N+1) where N is the number of cores
# of your system. That will maximize the CPU usage (one task per
# CPU thread/core + a provision of one extra task when a process
# is blocked by an I/O operation.
```

Anything but “ok” after the `make` command has completed would mean there was an error during the build process. As we ran a parallel build because of the `-j` option, it is not always easy to retrieve the error message given the large volume of output produced by the build system.

In the case of issue, just restart `make`, but without the `-j` option this time. And the error should appear near the end of the output:

```
sh$ make
```

Finally, once the compilation has gone to the end, you can install your software to its location by running the command:

```
sh$ sudo make install
```

And test it:

```
sh$ /opt/node/bin/node --version
v8.1.1
```

### B. What if things go wrong while installing from source code?

What I’ve explained above is mostly what you can see on the “build instruction” page of a well-documented project. But given this article goal is to let you compile your first software from sources, it might worth taking the time to investigate some common issues. So, I will do the whole procedure again, but this time from a fresh and minimal Debian 9.0 and CentOS 7.0 systems. So you can see the error I encountered and how I solved them.

### From Debian 9.0 “Stretch”

```
itsfoss@debian:~$ git clone --depth 1 \
                             --branch v8.1.1 \
                             https://github.com/nodejs/node
-bash: git: command not found
```

This problem is quite easy to diagnosis and solve. Just install the `git` package:

```
itsfoss@debian:~$ sudo apt-get install git
```

```
itsfoss@debian:~$ git clone --depth 1 \
                             --branch v8.1.1 \
                             https://github.com/nodejs/node && echo ok
[...]
ok
```

```
itsfoss@debian:~/node$ sudo mkdir /opt/node-v8.1.1
itsfoss@debian:~/node$ sudo ln -sT node-v8.1.1 /opt/node
```

No problem here.

```
itsfoss@debian:~/node$ ./configure --prefix=/opt/node-v8.1.1/
WARNING: failed to autodetect C++ compiler version (CXX=g++)
WARNING: failed to autodetect C compiler version (CC=gcc)
Node.js configure error: No acceptable C compiler found!
        Please make sure you have a C compiler installed on your system and/or
        consider adjusting the CC environment variable if you installed
        it in a non-standard prefix.
```

Obviously, to compile a project, you need a compiler. NodeJS being written using the [C++ language][20], we need a C++ [compiler][21]. Here I will install `g++`, the GNU C++ compiler for that purpose:

```
itsfoss@debian:~/node$ sudo apt-get install g++
itsfoss@debian:~/node$ ./configure --prefix=/opt/node-v8.1.1/ && echo ok
[...]
ok
```

```
itsfoss@debian:~/node$ make -j9 && echo ok
-bash: make: command not found
```

One other missing tool. Same symptoms. Same solution:

```
itsfoss@debian:~/node$ sudo apt-get install make
itsfoss@debian:~/node$ make -j9 && echo ok
[...]
ok
```

```
itsfoss@debian:~/node$ sudo make install
[...]
itsfoss@debian:~/node$ /opt/node/bin/node --version
v8.1.1
```

Success!

Please notice: I’ve installed the various tools one by one to show how to diagnosis the compilation issues and to show you the typical solution to solve those issues. But if you search more about that topic or read other tutorials, you will discover that most distributions have “meta-packages” acting as an umbrella to install some or all the typical tools used for compiling a software. On Debian-based systems, you will probably encounter the [build-essentials][22]package for that purpose. And on Red-Hat-based distributions, that will be the  _“Development Tools”_  group.

### From CentOS 7.0

```
[itsfoss@centos ~]$ git clone --depth 1 \
                               --branch v8.1.1 \
                               https://github.com/nodejs/node
-bash: git: command not found
```

Command not found? Just install it using the `yum` package manager:

```
[itsfoss@centos ~]$ sudo yum install git
```

```
[itsfoss@centos ~]$ git clone --depth 1 \
                               --branch v8.1.1 \
                               https://github.com/nodejs/node && echo ok
[...]
ok
```

```
[itsfoss@centos ~]$ sudo mkdir /opt/node-v8.1.1
[itsfoss@centos ~]$ sudo ln -sT node-v8.1.1 /opt/node
```

```
[itsfoss@centos ~]$ cd node
[itsfoss@centos node]$ ./configure --prefix=/opt/node-v8.1.1/
WARNING: failed to autodetect C++ compiler version (CXX=g++)
WARNING: failed to autodetect C compiler version (CC=gcc)
Node.js configure error: No acceptable C compiler found!

        Please make sure you have a C compiler installed on your system and/or
        consider adjusting the CC environment variable if you installed
        it in a non-standard prefix.
```

You guess it: NodeJS is written using the C++ language, but my system lacks the corresponding compiler. Yum to the rescue. As I’m not a regular CentOS user, I actually had to search on the Internet the exact name of the package containing the g++ compiler. Leading me to that page: [https://superuser.com/questions/590808/yum-install-gcc-g-doesnt-work-anymore-in-centos-6-4][23]

```
[itsfoss@centos node]$ sudo yum install gcc-c++
[itsfoss@centos node]$ ./configure --prefix=/opt/node-v8.1.1/ && echo ok
[...]
ok
```

```
[itsfoss@centos node]$ make -j9 && echo ok
[...]
ok
```

```
[itsfoss@centos node]$ sudo make install && echo ok
[...]
ok
```

```
[itsfoss@centos node]$ /opt/node/bin/node --version
v8.1.1
```

Success. Again.

### C. Making changes to the software installed from source code

You may install a software from the source because you need a very specific version not available in your distribution repository. Or because you want to  _modify_  that program. Either to fix a bug or add a feature. After all, open-source is all about that. So I will take that opportunity to give you a taste of the power you have at hand now you are able to compile your own software.

Here, we will make a minor change to the sources of NodeJS. And we will see if our change will be incorporated into the compiled version of the software:

Open the file `node/src/node.cc` in your favorite [text editor][24] (vim, nano, gedit, … ). And try to locate that fragment of code:

```
   if (debug_options.ParseOption(argv[0], arg)) {
      // Done, consumed by DebugOptions::ParseOption().
    } else if (strcmp(arg, "--version") == 0 || strcmp(arg, "-v") == 0) {
      printf("%s\n", NODE_VERSION);
      exit(0);
    } else if (strcmp(arg, "--help") == 0 || strcmp(arg, "-h") == 0) {
      PrintHelp();
      exit(0);
    }
```

It is around [line 3830 of the file][25]. Then modify the line containing `printf` to match that one instead:

```
      printf("%s (compiled by myself)\n", NODE_VERSION);
```

Then head back to your terminal. Before going further— and to give you some more insight of the power behind git— you can check if you’ve modified the right file:

```
diff --git a/src/node.cc b/src/node.cc
index bbce1022..a5618b57 100644
--- a/src/node.cc
+++ b/src/node.cc
@@ -3828,7 +3828,7 @@ static void ParseArgs(int* argc,
     if (debug_options.ParseOption(argv[0], arg)) {
       // Done, consumed by DebugOptions::ParseOption().
     } else if (strcmp(arg, "--version") == 0 || strcmp(arg, "-v") == 0) {
-      printf("%s\n", NODE_VERSION);
+      printf("%s (compiled by myself)\n", NODE_VERSION);
       exit(0);
     } else if (strcmp(arg, "--help") == 0 || strcmp(arg, "-h") == 0) {
       PrintHelp();
```

You should see a “-” (minus sign) before the line as it was before you changed it. And a “+” (plus sign) before the line after your changes.

It is now time to recompile and re-install your software:

```
make -j9 && sudo make install && echo ok
[...]
ok
```

This times, the only reason it might fail is that you’ve made a typo while changing the code. If this is the case, re-open the `node/src/node.cc` file in your text editor and fix the mistake.

Once you’ve managed to compile and install that new modified NodeJS version, you will be able to check if your modifications were actually incorporated into the software:

```
itsfoss@debian:~/node$ /opt/node/bin/node --version
v8.1.1 (compiled by myself)
```

Congratulations! You’ve made your first change to an open-source program!

### D. Let the shell locate our custom build software

You may have noticed until now, I always launched my newly compiled NodeJS software by specifying the absolute path to the binary file.

```
/opt/node/bin/node
```

It works. But this is annoying, to say the least. There are actually two common ways of fixing that. But to understand them, you must first know your shell locates the executable files by looking for them only into the directories specified by the `PATH` [environment variable][26].

```
itsfoss@debian:~/node$ echo $PATH
/usr/local/bin:/usr/bin:/bin:/usr/local/games:/usr/games
```

Here, on that Debian system, if you do not specify explicitly any directory as part of a command name, the shell will first look for that executable programs into `/usr/local/bin`, then if not found into `/usr/bin`, then if not found into `/bin` then if not found into `/usr/local/games` then if not found into `/usr/games`, then if not found … the shell will report an error  _“command not found”_ .

Given that, we have two way to make a command accessible to the shell: by adding it to one of the already configured `PATH` directories. Or by adding the directory containing our executable file to the `PATH`.

### Adding a link from /usr/local/bin

Just  _copying_  the node binary executable from `/opt/node/bin` to `/usr/local/bin` would be a bad idea since by doing so, the executable program would no longer be able to locate the other required components belonging to `/opt/node/` (it’s a common practice for a software to locate its resource files relative to its own location).

So, the traditional way of doing that is by using a symbolic link:

```
itsfoss@debian:~/node$ sudo ln -sT /opt/node/bin/node /usr/local/bin/node
itsfoss@debian:~/node$ which -a node || echo not found
/usr/local/bin/node
itsfoss@debian:~/node$ node --version
v8.1.1 (compiled by myself)
```

This is a simple and effective solution, especially if a software package is made of just few well known executable programs— since you have to create a symbolic link for each and every user-invokable commands. For example, if you’re familiar with NodeJS, you know the `npm` companion application I should symlink from `/usr/local/bin` too. But I let that to you as an exercise.

### Modifying the PATH

First, if you tried the preceding solution, remove the node symbolic link created previously to start from a clear state:

```
itsfoss@debian:~/node$ sudo rm /usr/local/bin/node
itsfoss@debian:~/node$ which -a node || echo not found
not found
```

And now, here is the magic command to change your `PATH`:

```
itsfoss@debian:~/node$ export PATH="/opt/node/bin:${PATH}"
itsfoss@debian:~/node$ echo $PATH
/opt/node/bin:/usr/local/bin:/usr/bin:/bin:/usr/local/games:/usr/games
```

Simply said, I replaced the content of the `PATH` environment variable by its previous content, but prefixed by `/opt/node/bin`. So, as you can imagine it now, the shell will look first into the `/opt/node/bin` directory for executable programs. We can confirm that using the `which` command:

```
itsfoss@debian:~/node$ which -a node || echo not found
/opt/node/bin/node
itsfoss@debian:~/node$ node --version
v8.1.1 (compiled by myself)
```

Whereas the “link” solution is permanent as soon as you’ve created the symbolic link into `/usr/local/bin`, the `PATH` change is effective only into the current shell. I let you do some researches by yourself to know how to make changes of the `PATH` permanents. As a hint, it has to do with your “profile”. If you find the solution, don’t hesitate to share that with the other readers by using the comment section below!

### E. How to remove that newly installed software from source code

Since our custom compiled NodeJS software sits completely in the `/opt/node-v8.1.1` directory, removing that software is not more work than using the `rm` command to remove that directory:

```
sudo rm -rf /opt/node-v8.1.1
```

BEWARE: `sudo` and `rm -rf` are a dangerous cocktail! Always check your command twice before pressing the “enter” key. You won’t have any confirmation message and no undelete if you remove the wrong directory…

Then, if you’ve modified your `PATH`, you will have to revert those changes. Which is not complicated at all.

And if you’ve created links from `/usr/local/bin` you will have to remove them all:

```
itsfoss@debian:~/node$ sudo find /usr/local/bin \
                                 -type l \
                                 -ilname "/opt/node/*" \
                                 -print -delete
/usr/local/bin/node
```

### Wait? Where was the Dependency Hell?

As a final comment, if you read about compiling your own custom software, you might have heard about the [dependency hell][27]. This is a nickname for that annoying situation where before being able to successfully compile a software, you must first compile a pre-requisite library, which in its turn requires another library that might in its turn be incompatible with some other software you’ve already installed.

Part of the job of the package maintainers of your distribution is to actually resolve that dependency hell and to ensure the various software of your system are using compatible libraries and are installed in the right order.

In that article, I chose on purpose to install NodeJS as it virtually doesn’t have dependencies. I said “virtually” because, in fact, it  _has_  dependencies. But the source code of those dependencies are present in the source repository of the project (in the `node/deps` subdirectory), so you don’t have to download and install them manually before hand.

But if you’re interested in understanding more about that problem and learn how to deal with it, let me know that using the comment section below: that would be a great topic for a more advanced article!

--------------------------------------------------------------------------------

作者简介：

Engineer by Passion, Teacher by Vocation. My goals : to share my enthusiasm for what I teach and prepare my students to develop their skills by themselves. You can find me on my website as well.

--------------------

via: https://itsfoss.com/install-software-from-source-code/

作者：[Sylvain Leroux ][a]
译者：[译者ID](https://github.com/译者ID)
校对：[校对者ID](https://github.com/校对者ID)

本文由 [LCTT](https://github.com/LCTT/TranslateProject) 原创编译，[Linux中国](https://linux.cn/) 荣誉推出

[a]:https://itsfoss.com/author/sylvain/
[1]:https://itsfoss.com/author/sylvain/
[2]:https://itsfoss.com/install-software-from-source-code/#comments
[3]:https://www.facebook.com/share.php?u=https%3A%2F%2Fitsfoss.com%2Finstall-software-from-source-code%2F%3Futm_source%3Dfacebook%26utm_medium%3Dsocial%26utm_campaign%3DSocialWarfare
[4]:https://twitter.com/share?original_referer=/&text=How+to+Install+Software+from+Source+Code%E2%80%A6+and+Remove+it+Afterwards&url=https://itsfoss.com/install-software-from-source-code/%3Futm_source%3Dtwitter%26utm_medium%3Dsocial%26utm_campaign%3DSocialWarfare&via=Yes_I_Know_IT
[5]:https://plus.google.com/share?url=https%3A%2F%2Fitsfoss.com%2Finstall-software-from-source-code%2F%3Futm_source%3DgooglePlus%26utm_medium%3Dsocial%26utm_campaign%3DSocialWarfare
[6]:https://www.linkedin.com/cws/share?url=https%3A%2F%2Fitsfoss.com%2Finstall-software-from-source-code%2F%3Futm_source%3DlinkedIn%26utm_medium%3Dsocial%26utm_campaign%3DSocialWarfare
[7]:https://www.reddit.com/submit?url=https://itsfoss.com/install-software-from-source-code/&title=How+to+Install+Software+from+Source+Code%E2%80%A6+and+Remove+it+Afterwards
[8]:https://itsfoss.com/remove-install-software-ubuntu/
[9]:https://nodejs.org/en/
[10]:https://github.com/nodejs/node
[11]:https://en.wikipedia.org/wiki/GitHub
[12]:https://en.wikipedia.org/wiki/Git
[13]:https://en.wikipedia.org/wiki/Version_control
[14]:https://stackoverflow.com/questions/1457103/how-is-a-tag-different-from-a-branch-which-should-i-use-here
[15]:https://en.wikipedia.org/wiki/Graphical_user_interface
[16]:https://en.wikipedia.org/wiki/GNU_Build_System
[17]:https://en.wikipedia.org/wiki/Make_%28software
[18]:https://itsfoss.com/pro-vim-tips/
[19]:http://www.pathname.com/fhs/
[20]:https://en.wikipedia.org/wiki/C%2B%2B
[21]:https://en.wikipedia.org/wiki/Compiler
[22]:https://packages.debian.org/sid/build-essential
[23]:https://superuser.com/questions/590808/yum-install-gcc-g-doesnt-work-anymore-in-centos-6-4
[24]:https://en.wikipedia.org/wiki/List_of_text_editors
[25]:https://github.com/nodejs/node/blob/v8.1.1/src/node.cc#L3830
[26]:https://en.wikipedia.org/wiki/Environment_variable
[27]:https://en.wikipedia.org/wiki/Dependency_hell
