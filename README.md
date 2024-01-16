<h3>docker镜像拉取缓慢或无法拉取请自行配置加速</h3>
<p><a href="https://hub.docker.com/r/kissnavel/qsign">docker镜像地址</a></p>
<p>奇妙的Sign API</p>
<p>感谢<a href="https://github.com/fuqiuluo/unidbg-fetch-qsign">github.com/fuqiuluo/unidbg-fetch-qsign</a></p>
<p>仅支持linux/amd64，不支持linux/arm64</p>
<p>建议配置cron参数定时重启容器</p>
<p>windows平台请看<a href="https://gitee.com/touchscale/Qsign">这里</a></p>
<h3>pull tags</h3>
<p><code>core-1.1.9</code>将<a href="https://hub.docker.com/r/xzhouqd/qsign">core</a>与<a href="https://gitee.com/touchscale/Qsign/tree/master/unidbg-fetch-qsign/txlib">协议</a>打包，默认协议版本<code>8.9.80</code></p>
<p><code>touchscale-1.2.0</code>打包自<a href="https://gitee.com/touchscale/Qsign/tree/master/unidbg-fetch-qsign">Qsign</a>，仅小改下版本号，其余与<code>core-1.1.9</code>无区别</p>
<p><code>core-1.2.1</code>不推荐使用，此版本运行几十分钟或几小时后容器在接收调用时有概率卡死崩溃。虽然docker有<code>--restart=always</code>这个参数存在，可以在容器卡死崩溃时自动重启，但是会造成指令发出去不反馈或者过了1分钟左右才反馈的后果</p>
<h3>本镜像包含协议txlib情况</h3>
<p><code>3.5.1</code>、<code>3.5.2</code>、<code>8.9.63</code>、<code>8.9.68</code>、<code>8.9.70</code>、<code>8.9.71</code>、<code>8.9.73</code>、<code>8.9.75</code>、<code>8.9.76</code>、<code>8.9.78</code>、<code>8.9.80</code>、<code>8.9.83</code>、<code>8.9.85</code>、<code>8.9.88</code>、<code>8.9.90</code>、<code>8.9.93</code>、<code>9.0.0</code>、<code>9.0.8</code></p>
<p>镜像内的协议随<a href="https://gitee.com/touchscale/Qsign/tree/master/unidbg-fetch-qsign/txlib">此处</a>的更新而更新，更新镜像会覆盖原镜像，要使用最新镜像请删除本地镜像后重新拉取并部署</p>
<p>以8.9.80举例，<code>core-1.1.9</code>默认配置文件，请先检查是否确实是自己想要的配置</p>
<pre><code>{ 
   "server": { 
     "host": "0.0.0.0", 
     "port": 801 
   }, 
   "key": "114514", 
   "auto_register": true, 
   "protocol": { 
     "package_name": "com.tencent.mobileqq", 
     "qua": "V1_AND_SQ_8.9.80_4614_YYB_D", 
     "version": "8.9.80", 
     "code": "4614" 
   }, 
   "unidbg": {
    "dynarmic": false,
    "unicorn": true,
    "debug": false
  },
   "black_list": [
     1008611
   ]
 }</code></pre>
<h3>部署方式</h3>
<p>如需部署不同tag请自行替换下方部署方式中的pull tag</p>
<p>拉取镜像：</p>
<pre><code>docker pull kissnavel/qsign:core-1.1.9</code></pre>
<p><code>{host_port}</code>填你想要的宿主机上的端口号，如<code>801</code>，<code>{version}</code>填协议版本号，如<code>8.9.80</code></p>
<h4>1.使用默认配置文件，可使用如下简化命令运行镜像：</h4>
<p>部署镜像：</p>
<pre><code>docker run -d -p {host_port}:801 --restart=always --name qsign kissnavel/qsign:core-1.1.9</code></pre>
<h4>2.使用默认配置文件，指定协议版本：</h4>
<p>部署镜像：</p>
<pre><code>docker run -d -p {host_port}:801 --restart=always -e BASE_PATH=/srv/qsign/qsign/txlib/{version} --name qsign kissnavel/qsign:core-1.1.9</code></pre>
<h4>3.修改配置文件，传入config.json/整体传入一个txlib文件夹的部署方式：</h4>
<p>1.传入config.json，不修改其他内容：</p>
<pre><code>docker run -d -p {host_port}:{internal_port} --restart=always -e BASE_PATH=/srv/qsign/qsign/txlib/{version} -v {host_abs_config.json_path}:/srv/qsign/qsign/txlib/{version}/config.json --name qsign kissnavel/qsign:core-1.1.9</code></pre>
<pre><code>{host_port}: 宿主机侧访问的端口
{internal_port}: 容器内服务端口（在config.json配置！）
{host_abs_config.json_path}: 宿主机侧config.json文件绝对路径
{version}: 协议版本号，如8.9.80</code></pre>
<p>2.整体传入一个完整txlib文件夹：</p>
<pre><code>docker run -d -p {host_port}:{internal_port} --restart=always -e BASE_PATH={internal_abs_base_path} -v {host_abs_txlib_path}:{internal_abs_base_path} --name qsign kissnavel/qsign:core-1.1.9</code></pre>
<pre><code>{host_port}: 宿主机侧访问的端口
{internal_port}: 容器内服务端口（在config.json配置！）
{internal_abs_base_path}: 容器内txlib具体所在目录（包含4个文件的目录）绝对路径
{host_abs_txlib_path}: 宿主机侧txlib所在目录绝对路径</code></pre>
<h3>签名API地址</h3>
<p>默认地址：<code>http://127.0.0.1:801/sign?key=114514</code>，根据自己实际情况修改</p>
<h3>常见问题</h3>
<h4>1.Read memory failed</h4>
<p>目前使用9.0.0及以上协议会报错内存错误并强制退出，暂无有效解决方法，请使用低版本</p>
<h4>2.JAVA_HOME报错</h4>
<p>更新你的docker engine！eclipse temurin底包版本很高，当你的os和docker engine版本很低的时候，会报错，升级docker engine能解决这个问题</p>