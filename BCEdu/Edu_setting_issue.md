## 환경 세팅 이슈 정리
### 1. VM에서 apt repository 추가 후 apt update 수행시 404오류 발생
apt repository 추가시 잘못된 주소로 등록되어짐
```
정상 : 
$ sudo add-apt-repository  "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable"
비정상 : 
$ sudo add-apt-repository  "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
  (lsb_release -cs) stable"
```
/etc/apt/sources.list에서 잘못 등록되어진 주소 제거 후 apt update 수행

### 2. VM에서 enrolladmin.js 실행시 오류 발생
nodejs 버전이 맞지 않아서 발생

```
 $ node enrollAdmin.js
 module.js:328
     throw err;
     ^
 
 Error: Cannot find module 'fabric-client'
     at Function.Module._resolveFilename (module.js:326:15)
     at Function.Module._load (module.js:277:25)
     at Module.require (module.js:354:17)
     at require (internal/module.js:12:17)
     at Object.<anonymous> (/home/zing/fabric-samples/fabcar/enrollAdmin.js:11:21)
     at Module._compile (module.js:410:26)
     at Object.Module._extensions..js (module.js:417:10)
     at Module.load (module.js:344:32)
     at Function.Module._load (module.js:301:12)
     at Function.Module.runMain (module.js:442:10)
```
nodejs 최신버전 설치

```
$ sudo apt-get update && sudo apt-get –y upgrade
$ curl –sL https://deb.nodesource.com/setup_9.x | sudo -E bash – 
$ sudo apt-get install nodejs
$ npm install
```


### 3. Windows에서 npm install 수행시 오류 발생
install 순서에서 문제가 발생할 수 있음
```
C:\myapp>npm install
npm WARN deprecated crypto@1.0.1: This package is no longer supported. It's now a built-in Node module. If you've depended on crypto, you should switch to the one that's built-in.

> dtrace-provider@0.8.6 install C:\myapp\node_modules\dtrace-provider
> node-gyp rebuild || node suppress-error.js


C:\myapp\node_modules\dtrace-provider>if not defined npm_config_node_gyp (node "C:\Program Files\nodejs\node_modules\npm\node_modules\npm-lifecycle\node-gyp-bin\\..\..\node_modules\node-gyp\bin\node-gyp.js" rebuild )  else (node "C:\Program Files\nodejs\node_modules\npm\node_modules\node-gyp\bin\node-gyp.js" rebuild )
gyp ERR! configure error
gyp ERR! stack Error: Can't find Python executable "python", you can set the PYTHON env variable.
gyp ERR! stack     at PythonFinder.failNoPython (C:\Program Files\nodejs\node_modules\npm\node_modules\node-gyp\lib\configure.js:483:19)
gyp ERR! stack     at PythonFinder.<anonymous> (C:\Program Files\nodejs\node_modules\npm\node_modules\node-gyp\lib\configure.js:508:16)
gyp ERR! stack     at C:\Program Files\nodejs\node_modules\npm\node_modules\graceful-fs\polyfills.js:284:29
gyp ERR! stack     at FSReqWrap.oncomplete (fs.js:170:21)
gyp ERR! System Windows_NT 10.0.16299
gyp ERR! command "C:\\Program Files\\nodejs\\node.exe" "C:\\Program Files\\nodejs\\node_modules\\npm\\node_modules\\node-gyp\\bin\\node-gyp.js" "rebuild"
gyp ERR! cwd C:\myapp\node_modules\dtrace-provider
gyp ERR! node -v v9.8.0
gyp ERR! node-gyp -v v3.6.2
gyp ERR! not ok

> pkcs11js@1.0.13 install C:\myapp\node_modules\pkcs11js
> node-gyp rebuild


C:\myapp\node_modules\pkcs11js>if not defined npm_config_node_gyp (node "C:\Program Files\nodejs\node_modules\npm\node_modules\npm-lifecycle\node-gyp-bin\\..\..\node_modules\node-gyp\bin\node-gyp.js" rebuild )  else (node "C:\Program Files\nodejs\node_modules\npm\node_modules\node-gyp\bin\node-gyp.js" rebuild )
gyp ERR! configure error
gyp ERR! stack Error: Can't find Python executable "python", you can set the PYTHON env variable.
gyp ERR! stack     at PythonFinder.failNoPython (C:\Program Files\nodejs\node_modules\npm\node_modules\node-gyp\lib\configure.js:483:19)
gyp ERR! stack     at PythonFinder.<anonymous> (C:\Program Files\nodejs\node_modules\npm\node_modules\node-gyp\lib\configure.js:508:16)
gyp ERR! stack     at C:\Program Files\nodejs\node_modules\npm\node_modules\graceful-fs\polyfills.js:284:29
gyp ERR! stack     at FSReqWrap.oncomplete (fs.js:170:21)
gyp ERR! System Windows_NT 10.0.16299
gyp ERR! command "C:\\Program Files\\nodejs\\node.exe" "C:\\Program Files\\nodejs\\node_modules\\npm\\node_modules\\node-gyp\\bin\\node-gyp.js" "rebuild"
gyp ERR! cwd C:\myapp\node_modules\pkcs11js
gyp ERR! node -v v9.8.0
gyp ERR! node-gyp -v v3.6.2
gyp ERR! not ok
npm WARN myapp@1.0.0 No description
npm WARN myapp@1.0.0 No repository field.

npm ERR! code ELIFECYCLE
npm ERR! errno 1
npm ERR! pkcs11js@1.0.13 install: `node-gyp rebuild`
npm ERR! Exit status 1
npm ERR!
npm ERR! Failed at the pkcs11js@1.0.13 install script.
npm ERR! This is probably not a problem with npm. There is likely additional logging output above.

npm ERR! A complete log of this run can be found in:
npm ERR!     C:\Users\Administrator\AppData\Roaming\npm-cache\_logs\2018-03-20T15_09_58_062Z-debug.log
```
windows build tools 선 설치 후 npm install 재수행
```
C:\myapp>npm install windows-build-tools

> windows-build-tools@2.2.1 postinstall C:\myapp\node_modules\windows-build-tools
> node ./lib/index.js

Downloading BuildTools_Full.exe
Downloading python-2.7.14.amd64.msi
[>                                            ] 0.0% (0 B/s)
Downloaded python-2.7.14.amd64.msi. Saved to C:\Users\Administrator\.windows-build-tools\python-2.7.14.amd64.msi.

Starting installation...
Launched installers, now waiting for them to finish.
This will likely take some time - please be patient!

Status from the installers:
---------- Visual Studio Build Tools ----------
[3190:2AFC][2018-03-21T00:22:01]i325: Registering dependency: {a9528995-e130-4501-ae19-bbfaddb779cc} on package provider: {064E4EE4-4052-417B-A99F-CBB94C296235}, package: WinBlue_SDKStoreDXRem_x86_Patch
---------- Visual Studio Build Tools ----------
Successfully installed Visual Studio Build Tools.
------------------- Python --------------------
Successfully installed Python 2.7
npm WARN myapp@1.0.0 No description
npm WARN myapp@1.0.0 No repository field.

+ windows-build-tools@2.2.1
removed 66 packages and updated 2 packages in 315.372s
```
### 4. Windows에서 enrolladmin.js 실행시 오류 발생
nodejs 버전이 맞지 않아서 발생
```
C:\myapp>node enrollAdmin.js
module.js:545
    throw err;
    ^

Error: Cannot find module 'fabric-client'
    at Function.Module._resolveFilename (module.js:543:15)
    at Function.Module._load (module.js:470:25)
    at Module.require (module.js:593:17)
    at require (internal/module.js:11:18)
    at Object.<anonymous> (C:\myapp\enrollAdmin.js:11:21)
    at Module._compile (module.js:649:30)
    at Object.Module._extensions..js (module.js:660:10)
    at Module.load (module.js:561:32)
    at tryModuleLoad (module.js:501:12)
    at Function.Module._load (module.js:493:3)
```
npm rebuild, npm install을 수행하여 nodjs 최신버전 설치
```
C:\myapp>npm rebuild

> grpc@1.10.0 install C:\myapp\node_modules\grpc
> node-pre-gyp install --fallback-to-build --library=static_library

[grpc] Success: "C:\myapp\node_modules\grpc\src\node\extension_binary\node-v59-win32-x64-unknown\grpc_node.node" is installed via remote

> windows-build-tools@2.2.1 postinstall C:\myapp\node_modules\windows-build-tools
> node ./lib/index.js

Downloading BuildTools_Full.exe
Downloading python-2.7.14.amd64.msi
[>                                            ] 0.0% (0 B/s)
Downloaded python-2.7.14.amd64.msi. Saved to C:\Users\Administrator\.windows-build-tools\python-2.7.14.amd64.msi.

Starting installation...
Launched installers, now waiting for them to finish.
This will likely take some time - please be patient!

Status from the installers:
---------- Visual Studio Build Tools ----------
Successfully installed Visual Studio Build Tools.
------------------- Python --------------------
Successfully installed Python 2.7
crypto@1.0.1 C:\myapp\node_modules\crypto
express@4.16.3 C:\myapp\node_modules\express
accepts@1.3.5 C:\myapp\node_modules\accepts
mime-types@2.1.18 C:\myapp\node_modules\mime-types
mime-db@1.33.0 C:\myapp\node_modules\mime-db
negotiator@0.6.1 C:\myapp\node_modules\negotiator
array-flatten@1.1.1 C:\myapp\node_modules\array-flatten
body-parser@1.18.2 C:\myapp\node_modules\body-parser
bytes@3.0.0 C:\myapp\node_modules\bytes
content-type@1.0.4 C:\myapp\node_modules\content-type
debug@2.6.9 C:\myapp\node_modules\debug
ms@2.0.0 C:\myapp\node_modules\ms
depd@1.1.2 C:\myapp\node_modules\depd
http-errors@1.6.2 C:\myapp\node_modules\http-errors
depd@1.1.1 C:\myapp\node_modules\http-errors\node_modules\depd
inherits@2.0.3 C:\myapp\node_modules\inherits
setprototypeof@1.0.3 C:\myapp\node_modules\http-errors\node_modules\setprototypeof
statuses@1.4.0 C:\myapp\node_modules\statuses
iconv-lite@0.4.19 C:\myapp\node_modules\iconv-lite
on-finished@2.3.0 C:\myapp\node_modules\on-finished
ee-first@1.1.1 C:\myapp\node_modules\ee-first
qs@6.5.1 C:\myapp\node_modules\qs
raw-body@2.3.2 C:\myapp\node_modules\raw-body
unpipe@1.0.0 C:\myapp\node_modules\unpipe
type-is@1.6.16 C:\myapp\node_modules\type-is
media-typer@0.3.0 C:\myapp\node_modules\media-typer
content-disposition@0.5.2 C:\myapp\node_modules\content-disposition
cookie@0.3.1 C:\myapp\node_modules\cookie
cookie-signature@1.0.6 C:\myapp\node_modules\cookie-signature
encodeurl@1.0.2 C:\myapp\node_modules\encodeurl
escape-html@1.0.3 C:\myapp\node_modules\escape-html
etag@1.8.1 C:\myapp\node_modules\etag
finalhandler@1.1.1 C:\myapp\node_modules\finalhandler
parseurl@1.3.2 C:\myapp\node_modules\parseurl
fresh@0.5.2 C:\myapp\node_modules\fresh
merge-descriptors@1.0.1 C:\myapp\node_modules\merge-descriptors
methods@1.1.2 C:\myapp\node_modules\methods
path-to-regexp@0.1.7 C:\myapp\node_modules\path-to-regexp
proxy-addr@2.0.3 C:\myapp\node_modules\proxy-addr
forwarded@0.1.2 C:\myapp\node_modules\forwarded
ipaddr.js@1.6.0 C:\myapp\node_modules\ipaddr.js
range-parser@1.2.0 C:\myapp\node_modules\range-parser
safe-buffer@5.1.1 C:\myapp\node_modules\safe-buffer
send@0.16.2 C:\myapp\node_modules\send
destroy@1.0.4 C:\myapp\node_modules\destroy
mime@1.4.1 C:\myapp\node_modules\mime
serve-static@1.13.2 C:\myapp\node_modules\serve-static
setprototypeof@1.1.0 C:\myapp\node_modules\setprototypeof
utils-merge@1.0.1 C:\myapp\node_modules\utils-merge
vary@1.1.2 C:\myapp\node_modules\vary
fabric-ca-client@1.1.0 C:\myapp\node_modules\fabric-ca-client
bn.js@4.11.8 C:\myapp\node_modules\bn.js
elliptic@6.4.0 C:\myapp\node_modules\elliptic
brorand@1.1.0 C:\myapp\node_modules\brorand
hash.js@1.1.3 C:\myapp\node_modules\hash.js
minimalistic-assert@1.0.0 C:\myapp\node_modules\minimalistic-assert
hmac-drbg@1.0.1 C:\myapp\node_modules\hmac-drbg
minimalistic-crypto-utils@1.0.1 C:\myapp\node_modules\minimalistic-crypto-utils
fs-extra@0.30.0 C:\myapp\node_modules\fs-extra
graceful-fs@4.1.11 C:\myapp\node_modules\graceful-fs
jsonfile@2.4.0 C:\myapp\node_modules\jsonfile
klaw@1.3.1 C:\myapp\node_modules\klaw
path-is-absolute@1.0.1 C:\myapp\node_modules\path-is-absolute
rimraf@2.6.2 C:\myapp\node_modules\rimraf
glob@7.1.2 C:\myapp\node_modules\glob
fs.realpath@1.0.0 C:\myapp\node_modules\fs.realpath
inflight@1.0.6 C:\myapp\node_modules\inflight
once@1.4.0 C:\myapp\node_modules\once
wrappy@1.0.2 C:\myapp\node_modules\wrappy
minimatch@3.0.4 C:\myapp\node_modules\minimatch
brace-expansion@1.1.11 C:\myapp\node_modules\brace-expansion
balanced-match@1.0.0 C:\myapp\node_modules\balanced-match
concat-map@0.0.1 C:\myapp\node_modules\concat-map
js-sha3@0.5.7 C:\myapp\node_modules\js-sha3
jsrsasign@7.2.2 C:\myapp\node_modules\jsrsasign
jssha@2.3.1 C:\myapp\node_modules\jssha
long@3.2.0 C:\myapp\node_modules\long
nconf@0.8.5 C:\myapp\node_modules\nconf
async@1.5.2 C:\myapp\node_modules\async
ini@1.3.5 C:\myapp\node_modules\ini
secure-keys@1.0.0 C:\myapp\node_modules\secure-keys
yargs@3.32.0 C:\myapp\node_modules\yargs
camelcase@2.1.1 C:\myapp\node_modules\camelcase
cliui@3.2.0 C:\myapp\node_modules\cliui
string-width@1.0.2 C:\myapp\node_modules\string-width
code-point-at@1.1.0 C:\myapp\node_modules\code-point-at
is-fullwidth-code-point@1.0.0 C:\myapp\node_modules\is-fullwidth-code-point
number-is-nan@1.0.1 C:\myapp\node_modules\number-is-nan
strip-ansi@3.0.1 C:\myapp\node_modules\strip-ansi
ansi-regex@2.1.1 C:\myapp\node_modules\ansi-regex
wrap-ansi@2.1.0 C:\myapp\node_modules\wrap-ansi
decamelize@1.2.0 C:\myapp\node_modules\decamelize
os-locale@1.4.0 C:\myapp\node_modules\os-locale
lcid@1.0.0 C:\myapp\node_modules\lcid
invert-kv@1.0.0 C:\myapp\node_modules\invert-kv
window-size@0.1.4 C:\myapp\node_modules\window-size
y18n@3.2.1 C:\myapp\node_modules\y18n
sjcl@1.0.7 C:\myapp\node_modules\sjcl
sjcl-codec@0.1.1 C:\myapp\node_modules\sjcl-codec
url@0.11.0 C:\myapp\node_modules\url
punycode@1.3.2 C:\myapp\node_modules\punycode
querystring@0.2.0 C:\myapp\node_modules\querystring
util@0.10.3 C:\myapp\node_modules\util
inherits@2.0.1 C:\myapp\node_modules\util\node_modules\inherits
winston@2.4.1 C:\myapp\node_modules\winston
async@1.0.0 C:\myapp\node_modules\winston\node_modules\async
colors@1.0.3 C:\myapp\node_modules\colors
cycle@1.0.3 C:\myapp\node_modules\cycle
eyes@0.1.8 C:\myapp\node_modules\eyes
isstream@0.1.2 C:\myapp\node_modules\isstream
stack-trace@0.0.10 C:\myapp\node_modules\stack-trace
grpc@1.10.0 C:\myapp\node_modules\grpc
lodash@4.17.5 C:\myapp\node_modules\lodash
nan@2.10.0 C:\myapp\node_modules\nan
node-pre-gyp@0.6.39 C:\myapp\node_modules\grpc\node_modules\node-pre-gyp
detect-libc@1.0.3 C:\myapp\node_modules\grpc\node_modules\detect-libc
hawk@3.1.3 C:\myapp\node_modules\grpc\node_modules\hawk
boom@2.10.1 C:\myapp\node_modules\grpc\node_modules\boom
hoek@2.16.3 C:\myapp\node_modules\grpc\node_modules\hoek
cryptiles@2.0.5 C:\myapp\node_modules\grpc\node_modules\cryptiles
sntp@1.0.9 C:\myapp\node_modules\grpc\node_modules\sntp
mkdirp@0.5.1 C:\myapp\node_modules\grpc\node_modules\mkdirp
minimist@0.0.8 C:\myapp\node_modules\grpc\node_modules\mkdirp\node_modules\minimist
nopt@4.0.1 C:\myapp\node_modules\grpc\node_modules\nopt
abbrev@1.1.1 C:\myapp\node_modules\grpc\node_modules\abbrev
osenv@0.1.4 C:\myapp\node_modules\grpc\node_modules\osenv
os-homedir@1.0.2 C:\myapp\node_modules\grpc\node_modules\os-homedir
os-tmpdir@1.0.2 C:\myapp\node_modules\grpc\node_modules\os-tmpdir
npmlog@4.1.2 C:\myapp\node_modules\grpc\node_modules\npmlog
are-we-there-yet@1.1.4 C:\myapp\node_modules\grpc\node_modules\are-we-there-yet
delegates@1.0.0 C:\myapp\node_modules\grpc\node_modules\delegates
readable-stream@2.3.3 C:\myapp\node_modules\grpc\node_modules\readable-stream
core-util-is@1.0.2 C:\myapp\node_modules\grpc\node_modules\core-util-is
inherits@2.0.3 C:\myapp\node_modules\grpc\node_modules\inherits
isarray@1.0.0 C:\myapp\node_modules\grpc\node_modules\isarray
process-nextick-args@1.0.7 C:\myapp\node_modules\grpc\node_modules\process-nextick-args
safe-buffer@5.1.1 C:\myapp\node_modules\grpc\node_modules\safe-buffer
string_decoder@1.0.3 C:\myapp\node_modules\grpc\node_modules\string_decoder
util-deprecate@1.0.2 C:\myapp\node_modules\grpc\node_modules\util-deprecate
console-control-strings@1.1.0 C:\myapp\node_modules\grpc\node_modules\console-control-strings
gauge@2.7.4 C:\myapp\node_modules\grpc\node_modules\gauge
aproba@1.2.0 C:\myapp\node_modules\grpc\node_modules\aproba
has-unicode@2.0.1 C:\myapp\node_modules\grpc\node_modules\has-unicode
object-assign@4.1.1 C:\myapp\node_modules\grpc\node_modules\object-assign
signal-exit@3.0.2 C:\myapp\node_modules\grpc\node_modules\signal-exit
string-width@1.0.2 C:\myapp\node_modules\grpc\node_modules\string-width
code-point-at@1.1.0 C:\myapp\node_modules\grpc\node_modules\code-point-at
is-fullwidth-code-point@1.0.0 C:\myapp\node_modules\grpc\node_modules\is-fullwidth-code-point
number-is-nan@1.0.1 C:\myapp\node_modules\grpc\node_modules\number-is-nan
strip-ansi@3.0.1 C:\myapp\node_modules\grpc\node_modules\strip-ansi
ansi-regex@2.1.1 C:\myapp\node_modules\grpc\node_modules\ansi-regex
wide-align@1.1.2 C:\myapp\node_modules\grpc\node_modules\wide-align
set-blocking@2.0.0 C:\myapp\node_modules\grpc\node_modules\set-blocking
rc@1.2.4 C:\myapp\node_modules\grpc\node_modules\rc
deep-extend@0.4.2 C:\myapp\node_modules\grpc\node_modules\deep-extend
ini@1.3.5 C:\myapp\node_modules\grpc\node_modules\ini
minimist@1.2.0 C:\myapp\node_modules\grpc\node_modules\rc\node_modules\minimist
strip-json-comments@2.0.1 C:\myapp\node_modules\grpc\node_modules\strip-json-comments
request@2.81.0 C:\myapp\node_modules\grpc\node_modules\request
aws-sign2@0.6.0 C:\myapp\node_modules\grpc\node_modules\aws-sign2
aws4@1.6.0 C:\myapp\node_modules\grpc\node_modules\aws4
caseless@0.12.0 C:\myapp\node_modules\grpc\node_modules\caseless
combined-stream@1.0.5 C:\myapp\node_modules\grpc\node_modules\combined-stream
delayed-stream@1.0.0 C:\myapp\node_modules\grpc\node_modules\delayed-stream
extend@3.0.1 C:\myapp\node_modules\grpc\node_modules\extend
forever-agent@0.6.1 C:\myapp\node_modules\grpc\node_modules\forever-agent
form-data@2.1.4 C:\myapp\node_modules\grpc\node_modules\form-data
asynckit@0.4.0 C:\myapp\node_modules\grpc\node_modules\asynckit
mime-types@2.1.17 C:\myapp\node_modules\grpc\node_modules\mime-types
mime-db@1.30.0 C:\myapp\node_modules\grpc\node_modules\mime-db
har-validator@4.2.1 C:\myapp\node_modules\grpc\node_modules\har-validator
ajv@4.11.8 C:\myapp\node_modules\grpc\node_modules\ajv
co@4.6.0 C:\myapp\node_modules\grpc\node_modules\co
json-stable-stringify@1.0.1 C:\myapp\node_modules\grpc\node_modules\json-stable-stringify
jsonify@0.0.0 C:\myapp\node_modules\grpc\node_modules\jsonify
har-schema@1.0.5 C:\myapp\node_modules\grpc\node_modules\har-schema
http-signature@1.1.1 C:\myapp\node_modules\grpc\node_modules\http-signature
assert-plus@0.2.0 C:\myapp\node_modules\grpc\node_modules\assert-plus
jsprim@1.4.1 C:\myapp\node_modules\grpc\node_modules\jsprim
assert-plus@1.0.0 C:\myapp\node_modules\grpc\node_modules\jsprim\node_modules\assert-plus
extsprintf@1.3.0 C:\myapp\node_modules\grpc\node_modules\extsprintf
json-schema@0.2.3 C:\myapp\node_modules\grpc\node_modules\json-schema
verror@1.10.0 C:\myapp\node_modules\grpc\node_modules\verror
assert-plus@1.0.0 C:\myapp\node_modules\grpc\node_modules\verror\node_modules\assert-plus
sshpk@1.13.1 C:\myapp\node_modules\grpc\node_modules\sshpk
asn1@0.2.3 C:\myapp\node_modules\grpc\node_modules\asn1
assert-plus@1.0.0 C:\myapp\node_modules\grpc\node_modules\sshpk\node_modules\assert-plus
dashdash@1.14.1 C:\myapp\node_modules\grpc\node_modules\dashdash
assert-plus@1.0.0 C:\myapp\node_modules\grpc\node_modules\dashdash\node_modules\assert-plus
getpass@0.1.7 C:\myapp\node_modules\grpc\node_modules\getpass
assert-plus@1.0.0 C:\myapp\node_modules\grpc\node_modules\getpass\node_modules\assert-plus
is-typedarray@1.0.0 C:\myapp\node_modules\grpc\node_modules\is-typedarray
isstream@0.1.2 C:\myapp\node_modules\grpc\node_modules\isstream
json-stringify-safe@5.0.1 C:\myapp\node_modules\grpc\node_modules\json-stringify-safe
oauth-sign@0.8.2 C:\myapp\node_modules\grpc\node_modules\oauth-sign
performance-now@0.2.0 C:\myapp\node_modules\grpc\node_modules\performance-now
qs@6.4.0 C:\myapp\node_modules\grpc\node_modules\qs
stringstream@0.0.5 C:\myapp\node_modules\grpc\node_modules\stringstream
tough-cookie@2.3.3 C:\myapp\node_modules\grpc\node_modules\tough-cookie
punycode@1.4.1 C:\myapp\node_modules\grpc\node_modules\punycode
tunnel-agent@0.6.0 C:\myapp\node_modules\grpc\node_modules\tunnel-agent
uuid@3.2.1 C:\myapp\node_modules\grpc\node_modules\uuid
rimraf@2.6.2 C:\myapp\node_modules\grpc\node_modules\rimraf
glob@7.1.2 C:\myapp\node_modules\grpc\node_modules\glob
fs.realpath@1.0.0 C:\myapp\node_modules\grpc\node_modules\fs.realpath
inflight@1.0.6 C:\myapp\node_modules\grpc\node_modules\inflight
once@1.4.0 C:\myapp\node_modules\grpc\node_modules\once
wrappy@1.0.2 C:\myapp\node_modules\grpc\node_modules\wrappy
minimatch@3.0.4 C:\myapp\node_modules\grpc\node_modules\minimatch
brace-expansion@1.1.8 C:\myapp\node_modules\grpc\node_modules\brace-expansion
balanced-match@1.0.0 C:\myapp\node_modules\grpc\node_modules\balanced-match
concat-map@0.0.1 C:\myapp\node_modules\grpc\node_modules\concat-map
path-is-absolute@1.0.1 C:\myapp\node_modules\grpc\node_modules\path-is-absolute
semver@5.5.0 C:\myapp\node_modules\grpc\node_modules\semver
tar@2.2.1 C:\myapp\node_modules\grpc\node_modules\tar
block-stream@0.0.9 C:\myapp\node_modules\grpc\node_modules\block-stream
fstream@1.0.11 C:\myapp\node_modules\grpc\node_modules\fstream
graceful-fs@4.1.11 C:\myapp\node_modules\grpc\node_modules\graceful-fs
tar-pack@3.4.1 C:\myapp\node_modules\grpc\node_modules\tar-pack
debug@2.6.9 C:\myapp\node_modules\grpc\node_modules\debug
ms@2.0.0 C:\myapp\node_modules\grpc\node_modules\ms
fstream-ignore@1.0.5 C:\myapp\node_modules\grpc\node_modules\fstream-ignore
uid-number@0.0.6 C:\myapp\node_modules\grpc\node_modules\uid-number
protobufjs@5.0.2 C:\myapp\node_modules\protobufjs
ascli@1.0.1 C:\myapp\node_modules\ascli
colour@0.7.1 C:\myapp\node_modules\colour
optjs@3.2.2 C:\myapp\node_modules\optjs
bytebuffer@5.0.1 C:\myapp\node_modules\bytebuffer
bcrypt-pbkdf@1.0.1 C:\myapp\node_modules\grpc\node_modules\bcrypt-pbkdf
tweetnacl@0.14.5 C:\myapp\node_modules\grpc\node_modules\tweetnacl
ecc-jsbn@0.1.1 C:\myapp\node_modules\grpc\node_modules\ecc-jsbn
jsbn@0.1.1 C:\myapp\node_modules\grpc\node_modules\jsbn
windows-build-tools@2.2.1 C:\myapp\node_modules\windows-build-tools
chalk@2.3.2 C:\myapp\node_modules\chalk
ansi-styles@3.2.1 C:\myapp\node_modules\ansi-styles
color-convert@1.9.1 C:\myapp\node_modules\color-convert
color-name@1.1.3 C:\myapp\node_modules\color-name
escape-string-regexp@1.0.5 C:\myapp\node_modules\escape-string-regexp
supports-color@5.3.0 C:\myapp\node_modules\supports-color
has-flag@3.0.0 C:\myapp\node_modules\has-flag
debug@3.1.0 C:\myapp\node_modules\windows-build-tools\node_modules\debug
fs-extra@5.0.0 C:\myapp\node_modules\windows-build-tools\node_modules\fs-extra
jsonfile@4.0.0 C:\myapp\node_modules\windows-build-tools\node_modules\jsonfile
universalify@0.1.1 C:\myapp\node_modules\universalify
nugget@2.0.1 C:\myapp\node_modules\nugget
minimist@1.2.0 C:\myapp\node_modules\nugget\node_modules\minimist
pretty-bytes@1.0.4 C:\myapp\node_modules\pretty-bytes
get-stdin@4.0.1 C:\myapp\node_modules\get-stdin
meow@3.7.0 C:\myapp\node_modules\meow
camelcase-keys@2.1.0 C:\myapp\node_modules\camelcase-keys
map-obj@1.0.1 C:\myapp\node_modules\map-obj
loud-rejection@1.6.0 C:\myapp\node_modules\loud-rejection
currently-unhandled@0.4.1 C:\myapp\node_modules\currently-unhandled
array-find-index@1.0.2 C:\myapp\node_modules\array-find-index
signal-exit@3.0.2 C:\myapp\node_modules\signal-exit
minimist@1.2.0 C:\myapp\node_modules\meow\node_modules\minimist
normalize-package-data@2.4.0 C:\myapp\node_modules\normalize-package-data
hosted-git-info@2.6.0 C:\myapp\node_modules\hosted-git-info
is-builtin-module@1.0.0 C:\myapp\node_modules\is-builtin-module
builtin-modules@1.1.1 C:\myapp\node_modules\builtin-modules
semver@5.5.0 C:\myapp\node_modules\semver
validate-npm-package-license@3.0.3 C:\myapp\node_modules\validate-npm-package-license
spdx-correct@3.0.0 C:\myapp\node_modules\spdx-correct
spdx-expression-parse@3.0.0 C:\myapp\node_modules\spdx-expression-parse
spdx-exceptions@2.1.0 C:\myapp\node_modules\spdx-exceptions
spdx-license-ids@3.0.0 C:\myapp\node_modules\spdx-license-ids
object-assign@4.1.1 C:\myapp\node_modules\object-assign
read-pkg-up@1.0.1 C:\myapp\node_modules\read-pkg-up
find-up@1.1.2 C:\myapp\node_modules\find-up
path-exists@2.1.0 C:\myapp\node_modules\path-exists
pinkie-promise@2.0.1 C:\myapp\node_modules\pinkie-promise
pinkie@2.0.4 C:\myapp\node_modules\pinkie
read-pkg@1.1.0 C:\myapp\node_modules\read-pkg
load-json-file@1.1.0 C:\myapp\node_modules\load-json-file
parse-json@2.2.0 C:\myapp\node_modules\parse-json
error-ex@1.3.1 C:\myapp\node_modules\error-ex
is-arrayish@0.2.1 C:\myapp\node_modules\is-arrayish
pify@2.3.0 C:\myapp\node_modules\pify
strip-bom@2.0.0 C:\myapp\node_modules\strip-bom
is-utf8@0.2.1 C:\myapp\node_modules\is-utf8
path-type@1.1.0 C:\myapp\node_modules\path-type
redent@1.0.0 C:\myapp\node_modules\redent
indent-string@2.1.0 C:\myapp\node_modules\indent-string
repeating@2.0.1 C:\myapp\node_modules\repeating
is-finite@1.0.2 C:\myapp\node_modules\is-finite
strip-indent@1.0.1 C:\myapp\node_modules\strip-indent
trim-newlines@1.0.0 C:\myapp\node_modules\trim-newlines
progress-stream@1.2.0 C:\myapp\node_modules\progress-stream
speedometer@0.1.4 C:\myapp\node_modules\speedometer
through2@0.2.3 C:\myapp\node_modules\through2
readable-stream@1.1.14 C:\myapp\node_modules\through2\node_modules\readable-stream
core-util-is@1.0.2 C:\myapp\node_modules\core-util-is
isarray@0.0.1 C:\myapp\node_modules\through2\node_modules\isarray
string_decoder@0.10.31 C:\myapp\node_modules\through2\node_modules\string_decoder
xtend@2.1.2 C:\myapp\node_modules\through2\node_modules\xtend
object-keys@0.4.0 C:\myapp\node_modules\object-keys
request@2.85.0 C:\myapp\node_modules\request
aws-sign2@0.7.0 C:\myapp\node_modules\aws-sign2
aws4@1.6.0 C:\myapp\node_modules\aws4
caseless@0.12.0 C:\myapp\node_modules\caseless
combined-stream@1.0.6 C:\myapp\node_modules\combined-stream
delayed-stream@1.0.0 C:\myapp\node_modules\delayed-stream
extend@3.0.1 C:\myapp\node_modules\extend
forever-agent@0.6.1 C:\myapp\node_modules\forever-agent
form-data@2.3.2 C:\myapp\node_modules\form-data
asynckit@0.4.0 C:\myapp\node_modules\asynckit
har-validator@5.0.3 C:\myapp\node_modules\har-validator
ajv@5.5.2 C:\myapp\node_modules\ajv
co@4.6.0 C:\myapp\node_modules\co
fast-deep-equal@1.1.0 C:\myapp\node_modules\fast-deep-equal
fast-json-stable-stringify@2.0.0 C:\myapp\node_modules\fast-json-stable-stringify
json-schema-traverse@0.3.1 C:\myapp\node_modules\json-schema-traverse
har-schema@2.0.0 C:\myapp\node_modules\har-schema
hawk@6.0.2 C:\myapp\node_modules\hawk
boom@4.3.1 C:\myapp\node_modules\boom
hoek@4.2.1 C:\myapp\node_modules\hoek
cryptiles@3.1.2 C:\myapp\node_modules\cryptiles
boom@5.2.0 C:\myapp\node_modules\cryptiles\node_modules\boom
sntp@2.1.0 C:\myapp\node_modules\sntp
http-signature@1.2.0 C:\myapp\node_modules\http-signature
assert-plus@1.0.0 C:\myapp\node_modules\assert-plus
jsprim@1.4.1 C:\myapp\node_modules\jsprim
extsprintf@1.3.0 C:\myapp\node_modules\extsprintf
json-schema@0.2.3 C:\myapp\node_modules\json-schema
verror@1.10.0 C:\myapp\node_modules\verror
sshpk@1.14.1 C:\myapp\node_modules\sshpk
asn1@0.2.3 C:\myapp\node_modules\asn1
dashdash@1.14.1 C:\myapp\node_modules\dashdash
getpass@0.1.7 C:\myapp\node_modules\getpass
is-typedarray@1.0.0 C:\myapp\node_modules\is-typedarray
json-stringify-safe@5.0.1 C:\myapp\node_modules\json-stringify-safe
oauth-sign@0.8.2 C:\myapp\node_modules\oauth-sign
performance-now@2.1.0 C:\myapp\node_modules\performance-now
stringstream@0.0.5 C:\myapp\node_modules\stringstream
tough-cookie@2.3.4 C:\myapp\node_modules\tough-cookie
punycode@1.4.1 C:\myapp\node_modules\tough-cookie\node_modules\punycode
tunnel-agent@0.6.0 C:\myapp\node_modules\tunnel-agent
uuid@3.2.1 C:\myapp\node_modules\uuid
single-line-log@1.1.2 C:\myapp\node_modules\single-line-log
throttleit@0.0.2 C:\myapp\node_modules\throttleit
string-width@2.1.1 C:\myapp\node_modules\windows-build-tools\node_modules\string-width
is-fullwidth-code-point@2.0.0 C:\myapp\node_modules\windows-build-tools\node_modules\is-fullwidth-code-point
strip-ansi@4.0.0 C:\myapp\node_modules\windows-build-tools\node_modules\strip-ansi
ansi-regex@3.0.0 C:\myapp\node_modules\windows-build-tools\node_modules\ansi-regex
bcrypt-pbkdf@1.0.1 C:\myapp\node_modules\bcrypt-pbkdf
tweetnacl@0.14.5 C:\myapp\node_modules\tweetnacl
ecc-jsbn@0.1.1 C:\myapp\node_modules\ecc-jsbn
jsbn@0.1.1 C:\myapp\node_modules\jsbn
```
```
C:\myapp>npm install

> dtrace-provider@0.8.6 install C:\myapp\node_modules\dtrace-provider
> node-gyp rebuild || node suppress-error.js


C:\myapp\node_modules\dtrace-provider>if not defined npm_config_node_gyp (node "C:\Program Files\nodejs\node_modules\npm\node_modules\npm-lifecycle\node-gyp-bin\\..\..\node_modules\node-gyp\bin\node-gyp.js" rebuild )  else (node "C:\Program Files\nodejs\node_modules\npm\node_modules\node-gyp\bin\node-gyp.js" rebuild )
이 솔루션의 프로젝트를 한 번에 하나씩 빌드합니다. 병렬 빌드를 사용하려면 "/m" 스위치를 추가하십시오.

> pkcs11js@1.0.13 install C:\myapp\node_modules\pkcs11js
> node-gyp rebuild


C:\myapp\node_modules\pkcs11js>if not defined npm_config_node_gyp (node "C:\Program Files\nodejs\node_modules\npm\node_modules\npm-lifecycle\node-gyp-bin\\..\..\node_modules\node-gyp\bin\node-gyp.js" rebuild )  else (node "C:\Program Files\nodejs\node_modules\npm\node_modules\node-gyp\bin\node-gyp.js" rebuild )
이 솔루션의 프로젝트를 한 번에 하나씩 빌드합니다. 병렬 빌드를 사용하려면 "/m" 스위치를 추가하십시오.
  main.cpp
  dl.cpp
  const.cpp
  error.cpp
  v8_convert.cpp
  template.cpp
  mech.cpp
  param.cpp
  param_aes.cpp
  param_rsa.cpp
  param_ecdh.cpp
  pkcs11.cpp
  async.cpp
  node.cpp
  win_delay_load_hook.cc
..\src\async.cpp(31): warning C4996: 'Nan::Callback::Call': was declared deprecated [C:\myapp\node_modules\pkcs11js\build\pkcs11.vcxproj]
  C:\myapp\node_modules\nan\nan.h(1618): note: see declaration of 'Nan::Callback::Call'
..\src\async.cpp(55): warning C4996: 'Nan::Callback::Call': was declared deprecated [C:\myapp\node_modules\pkcs11js\build\pkcs11.vcxproj]
  C:\myapp\node_modules\nan\nan.h(1618): note: see declaration of 'Nan::Callback::Call'
..\src\async.cpp(100): warning C4996: 'Nan::Callback::Call': was declared deprecated [C:\myapp\node_modules\pkcs11js\build\pkcs11.vcxproj]
  C:\myapp\node_modules\nan\nan.h(1618): note: see declaration of 'Nan::Callback::Call'
..\src\async.cpp(124): warning C4996: 'Nan::Callback::Call': was declared deprecated [C:\myapp\node_modules\pkcs11js\build\pkcs11.vcxproj]
  C:\myapp\node_modules\nan\nan.h(1618): note: see declaration of 'Nan::Callback::Call'
..\src\async.cpp(148): warning C4996: 'Nan::Callback::Call': was declared deprecated [C:\myapp\node_modules\pkcs11js\build\pkcs11.vcxproj]
  C:\myapp\node_modules\nan\nan.h(1618): note: see declaration of 'Nan::Callback::Call'
..\src\async.cpp(172): warning C4996: 'Nan::Callback::Call': was declared deprecated [C:\myapp\node_modules\pkcs11js\build\pkcs11.vcxproj]
  C:\myapp\node_modules\nan\nan.h(1618): note: see declaration of 'Nan::Callback::Call'
     Creating library C:\myapp\node_modules\pkcs11js\build\Release\pkcs11.lib and object C:\myapp\node_modules\pkcs11js\build\Release\pkcs11.exp
  Generating code
  All 998 functions were compiled because no usable IPDB/IOBJ from previous compilation was found.
  Finished generating code
  pkcs11.vcxproj -> C:\myapp\node_modules\pkcs11js\build\Release\\pkcs11.node
  pkcs11.vcxproj -> C:\myapp\node_modules\pkcs11js\build\Release\pkcs11.pdb (Full PDB)
npm WARN myapp@1.0.0 No description
npm WARN myapp@1.0.0 No repository field.

added 69 packages in 38.172s
```

### 5. npm, nodejs 버전 때문에 문제가 생길 경우 
  - ```npm rebuild```, ```npm install``` 을 수행 후에도 오류가 계속 발생 할 경우 npm cache 삭제가 필요
  ```
  sudo npm cache clean -f
  ```
  - chache clean 이후 ```npm install``` 실행
