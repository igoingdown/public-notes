## 安装提示

```
To use the bundled libc++ please add the following LDFLAGS:
  LDFLAGS="-L/usr/local/opt/llvm/lib -Wl,-rpath,/usr/local/opt/llvm/lib"

This formula is keg-only, which means it was not symlinked into /usr/local,
because macOS already provides this software and installing another version in
parallel can cause all kinds of trouble.

If you need to have this software first in your PATH run:
  echo 'export PATH="/usr/local/opt/llvm/bin:$PATH"' >> ~/.bash_profile

For compilers to find this software you may need to set:

​```
LDFLAGS:  -L/usr/local/opt/llvm/lib
CPPFLAGS: -I/usr/local/opt/llvm/include
​```

If you need Python to find bindings for this keg-only formula, run:
  echo /usr/local/opt/llvm/lib/python2.7/site-packages >> /usr/local/lib/python2.7/site-packages/llvm.pth
  mkdir -p /Users/zhaomingxing/.local/lib/python2.7/site-packages
  echo 'import site; site.addsitedir("/usr/local/lib/python2.7/site-packages")' >> /Users/zhaomingxing/.local/lib/python2.7/site-packages/homebrew.pth
```








