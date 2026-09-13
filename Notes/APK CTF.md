---
categories:
  - "[[Labs]]"
topics:
  - "[[Mobile]]"
created:
---


```shell
─$ apktool d CTF3-0.apk
Picked up _JAVA_OPTIONS: -Dawt.useSystemAAFontSettings=on -Dswing.aatext=true
I: Using Apktool 2.7.0-dirty on CTF3-0.apk
I: Loading resource table...
I: Decoding AndroidManifest.xml with resources...
I: Loading resource table from file: /home/kali/.local/share/apktool/framework/1.apk
I: Regular manifest package...
I: Decoding file-resources...
I: Decoding values */* XMLs...
I: Baksmaling classes.dex...
I: Copying assets and libs...
I: Copying unknown files...
I: Copying original files...
```


**Searched for `Cipher\.getInstance\(.*?\)` and returned :**
```shell
    public final View u(LayoutInflater layoutInflater, ViewGroup viewGroup) {  
        byte[] J;  
        a aVar = (a) new s(this).e(a.class);  
        View inflate = layoutInflater.inflate(R.layout.fragment_notifications, viewGroup, false);  
        TextView textView = (TextView) i1.a.q(inflate, R.id.text_notifications);  
        if (textView == null) {  
            throw new NullPointerException("Missing required view with ID: ".concat(inflate.getResources().getResourceName(R.id.text_notifications)));  
        }  
        ConstraintLayout constraintLayout = (ConstraintLayout) inflate;  
        aVar.f2945d.d(l(), new r2.a(textView));  
        String Fragment = Fragment("15539863");  
        try {  
            J = J(F(), F().getString(R.string.app_logo));  
        } catch (IOException | ReflectiveOperationException | SecurityException | GeneralSecurityException unused) {  
        }  
        if (J.length <= 71612) {  
            return constraintLayout;  
        }  
        byte[] copyOfRange = Arrays.copyOfRange(J, 71612, J.length);  
        SecretKeySpec secretKeySpec = new SecretKeySpec(Fragment.getBytes(), "AES");  
        Cipher cipher = Cipher.getInstance("AES/ECB/PKCS5Padding");  
        cipher.init(2, secretKeySpec);  
        byte[] doFinal = cipher.doFinal(copyOfRange);  
        ByteBuffer allocateDirect = ByteBuffer.allocateDirect(doFinal.length);  
        allocateDirect.put(doFinal);  
        allocateDirect.position(0);  
        for (int i4 = 0; i4 < 2; i4++) {  
        }  
        Class<?> cls = Class.forName(K(V));  
        ((Class) cls.getMethod(K(W), String.class).invoke(cls.getConstructor(ByteBuffer.class, ClassLoader.class).newInstance(allocateDirect, getClass().getClassLoader()), K(X))).getMethod(K(Y), null).invoke(null, null);  
        return constraintLayout;  
    }
```

Upon inspect it has AES key `15539863` also loads data with `app_logo`

Checking the `res/values/strings.xml` we also see `app_logo`:

```bash
<string name="app_logo">google.png</string>

J = J(F(), F().getString(R.string.app_logo));
```

That last line shows the `google.png` file may contain embedded data. Looking into the file:

```shell
$ binwalk google.png

DECIMAL       HEXADECIMAL     DESCRIPTION
--------------------------------------------------------------------------------
0             0x0             PNG image, 800 x 800, 8-bit/color RGBA, non-interlaced
54            0x36            Zlib compressed data, compressed

```


```shell
$ binwalk -e google.png

DECIMAL       HEXADECIMAL     DESCRIPTION
--------------------------------------------------------------------------------
54            0x36            Zlib compressed data, compressed

WARNING: One or more files failed to extract: either no utility was found or it's unimplemented

                                                                                                                                                        
┌──(kali㉿kali)-[~/Downloads/CTF3-0/assets]
└─$ ls
dexopt  google.png  _google.png.extracted
                                                                                                                                                        
┌──(kali㉿kali)-[~/Downloads/CTF3-0/assets]
└─$ cd _*    

$ ls -la         
total 80
drwxrwxr-x 2 kali kali  4096 Mar 11 23:15 .
drwxrwxr-x 4 kali kali  4096 Mar 11 23:15 ..
-rw-rw-r-- 1 kali kali     0 Mar 11 23:15 36
-rw-rw-r-- 1 kali kali 73286 Mar 11 23:15 36.zlib
                                                    
```


```shell
$ zlib-flate -uncompress < 36.zlib > output
                                                                                                                                                        
┌──(kali㉿kali)-[~/Downloads/CTF3-0/assets/_google.png.extracted]
└─$ file output 
output: data

```