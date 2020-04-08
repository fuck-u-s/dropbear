# dropbear
dropbear最新版本编译成android可用的命令

下载源码  
wget https://matt.ucc.asn.au/dropbear/dropbear-2019.78.tar.bz2

解压
tar   -jxvf    dropbear-2019.78.tar.bz2

cd dropbear-2019.78/


编译命令参考 https://github.com/scue/android_dropbear_sshd

安装依赖包
sudo apt-get install gcc-arm-linux-gnueabi

配置,注意再后面添加静态编译配置

./configure --host=arm-linux-eabi --disable-zlib --disable-largefile --disable-loginfunc --disable-shadow --disable-utmp --disable-utmpx --disable-wtmp --disable-wtmpx --disable-pututline --disable-pututxline --disable-lastlog CC=arm-linux-gnueabi-gcc STRIP=arm-linux-gnueabi-strip LDFLAGS=-static
    
编译

export STATIC=1 MULTI=1 CC=arm-linux-gnueabi-gcc SCPPROGRESS=0 PROGRAMS="dropbear dropbearkey dbclient ssh"

make clean && make -j4 strip

最后一步


在上面输出的日志里找到最后一段链接成dropbearmulti的日志，如：

arm-linux-gnueabi-gcc -static -Wl,-pie -Wl,-z,now -Wl,-z,relro -o dropbearmulti dbmulti.o atomicio.o bignum.o buffer.o circbuffer.o cli-agentfwd.o cli-auth.o cli-authinteract.o cli-authpasswd.o cli-authpubkey.o cli-channel.o cli-chansession.o cli-kex.o cli-main.o cli-runopts.o cli-session.o cli-tcpfwd.o common-algo.o common-channel.o common-chansession.o common-kex.o common-runopts.o common-session.o compat.o crypto_desc.o curve25519-donna.o dbhelpers.o dbmalloc.o dbrandom.o dbutil.o dh_groups.o dropbearkey.o dss.o ecc.o ecdsa.o fake-rfc2553.o gendss.o genrsa.o gensignkey.o list.o listener.o loginrec.o ltc_prng.o netio.o packet.o process-packet.o queue.o rsa.o signkey.o sshpty.o svr-agentfwd.o svr-auth.o svr-authpam.o svr-authpasswd.o svr-authpubkey.o svr-authpubkeyoptions.o svr-chansession.o svr-kex.o svr-main.o svr-runopts.o svr-service.o svr-session.o svr-tcpfwd.o svr-x11fwd.o tcp-accept.o termcodes.o libtomcrypt/libtomcrypt.a libtommath/libtommath.a -lutil  -lcrypt

修改上面找到的命令，去掉动态链接配置，如：

arm-linux-gnueabi-gcc -static -o dropbearmulti dbmulti.o atomicio.o bignum.o buffer.o circbuffer.o cli-agentfwd.o cli-auth.o cli-authinteract.o cli-authpasswd.o cli-authpubkey.o cli-channel.o cli-chansession.o cli-kex.o cli-main.o cli-runopts.o cli-session.o cli-tcpfwd.o common-algo.o common-channel.o common-chansession.o common-kex.o common-runopts.o common-session.o compat.o crypto_desc.o curve25519-donna.o dbhelpers.o dbmalloc.o dbrandom.o dbutil.o dh_groups.o dropbearkey.o dss.o ecc.o ecdsa.o fake-rfc2553.o gendss.o genrsa.o gensignkey.o list.o listener.o loginrec.o ltc_prng.o netio.o packet.o process-packet.o queue.o rsa.o signkey.o sshpty.o svr-agentfwd.o svr-auth.o svr-authpam.o svr-authpasswd.o svr-authpubkey.o svr-authpubkeyoptions.o svr-chansession.o svr-kex.o svr-main.o svr-runopts.o svr-service.o svr-session.o svr-tcpfwd.o svr-x11fwd.o tcp-accept.o termcodes.o libtomcrypt/libtomcrypt.a libtommath/libtommath.a -lutil  -lcrypt

把上面得到的命令再执行一次，所得dropbearmulti即为编译结果









