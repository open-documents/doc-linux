
yum配置文件默认是`/etc/yum.conf`。下面是该配置文件的内容：
```text
[main]
cachedir=/var/cache/yum/$basearch/$releasever
keepcache=0
debuglevel=2
logfile=/var/log/yum.log
exactarch=1
obsoletes=1
gpgcheck=1
plugins=1
installonly_limit=5
bugtracker_url=http://bugs.centos.org/set_project.php?project_id=23&ref=http://bugs.centos.org/bug_report_page.php?category=yum
distroverpkg=centos-release


#  This is the default, if you make this bigger yum won't see if the metadata
# is newer on the remote and so you'll "gain" the bandwidth of not having to
# download the new metadata and "pay" for it by yum not having correct
# information.
#  It is esp. important, to have correct metadata, for distributions like
# Fedora which don't keep old packages around. If you don't like this checking
# interupting your command line usage, it's much better to have something
# manually check the metadata once an hour (yum-updatesd will do this).
# metadata_expire=90m

# PUT YOUR REPOS HERE OR IN separate files named file.repo
# in /etc/yum.repos.d

```

配置参数简要说明：
- cachedir：yum下载的RPM包的缓存目录。
- keepcache：缓存是否保存。1保存，0不保存。
- debuglevel：调试级别(0-10)，默认为2。
- logfile：日志文件。默认是 `/var/log/yum.log` 。  
- exactarch：更新时，是否允许更新不同版本的RPM包，比如是否在i386上更新i686的RPM包
- obsoletes：具体请参阅yum(8)，简单的说就是相当于upgrade，允许更新陈旧的RPM包。
- gpgchkeck：是否进行gpg校验。有1和0两个选择。这个选项如果设置在 `[main]` 部分，则对每个repository都有效。
- plugins：是否允许使用插件，默认是0不允许。但是一般会用yum-fastestmirror插件。
- installonly_limit：允许保留多少个内核包。
- exclude：屏蔽不想更新的RPM包。可用通配符，多个RPM包之间使用空格分离。


