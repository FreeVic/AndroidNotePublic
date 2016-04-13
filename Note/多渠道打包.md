Mass
===
##1.修改手机hosts
1.获取权限

	adb shell
	$ su  
	mount -o remount,rw /system 

2.覆盖hosts文件

	adb push  d:\hosts /system/etc/hosts

##2.美团自动化打包
解压apk压缩包，如果在META-INF目录内添加空文件，可以不用重新签名应用。因此，通过为不同渠道的应用添加不同的空文件，可以唯一标识一个渠道。

1.python脚本添加渠道名的前缀为ecchannel_

	#!usr/bin/python
	import zipfile
	import os
	import shutil
	def readChannelfile(filename):
	    f = open(filename,'r')
	    while True:
	        line = f.readline().strip('\n')
	        if len(line) == 0:
	            break
	        else:
	            list.append(line);
	    f.close()
	def init():
		open('./file.txt','w')
		if os.path.exists('./bin'):
			shutil.rmtree('./bin')
			print('delete bin')
		os.mkdir('./bin')
	list = [];
	readChannelfile('./channel')
	init()
	i = 0
	for name in list:
		outname = './bin/'+'one_'+name+'.apk'
		shutil.copyfile('./1.apk',outname)
		zipped = zipfile.ZipFile(outname, 'a', zipfile.ZIP_DEFLATED)
		empty_channel_file = "META-INF/ecchannel_{channel}".format(channel=name)
		zipped.write('./file.txt',empty_channel_file)
		zipped.close()
		i+=1
		print ('循环执行',i)

2.Java代码中读取空渠道文件名

	public static String getChannel(Context context) {
	        ApplicationInfo appinfo = context.getApplicationInfo();
	        String sourceDir = appinfo.sourceDir;
	        String ret = "";
	        ZipFile zipfile = null;
	        try {
	            zipfile = new ZipFile(sourceDir);
	            Enumeration<?> entries = zipfile.entries();
	            while (entries.hasMoreElements()) {
	                ZipEntry entry = ((ZipEntry) entries.nextElement());
	                String entryName = entry.getName();
	                if (entryName.startsWith("mtchannel")) {
	                    ret = entryName;
	                    break;
	                }
	            }
	        } catch (IOException e) {
	            e.printStackTrace();
	        } finally {
	            if (zipfile != null) {
	                try {
	                    zipfile.close();
	                } catch (IOException e) {
	                    e.printStackTrace();
	                }
	            }
	        }
	
	        String[] split = ret.split("_");
	        if (split != null && split.length >= 2) {
	            return ret.substring(split[0].length() + 1);
	
	        } else {
	            return "";
	        }
	    }

##3.Android Studio多渠道打包
1.友盟打包为例，清单文件配置

	<meta-data
	 android:name="UMENG_CHANNEL"
	 android:value="${UMENG_CHANNEL_VALUE}"/>
2.build.gradle中android节点下配置flavor

	 productFlavors {
        xiaomi {}
        _360 {}
        anzhi {}
        wandoujia {}
    }
	productFlavors.all {
	        flavor -> flavor.manifestPlaceholders = [UMENG_CHANNEL_VALUE: name]
	    }
	
3.studio 命令行中执行assemble命令：assemble+buildType中声明的类型

		如 gradlew assembleUnsignedBuild

4.完整build.gradle文件

	apply plugin: 'com.android.application'
	
	def releaseTime() {
	    return new Date().format("yyyy-MM-dd", TimeZone.getTimeZone("UTC"))
	}
	
	android {
	    compileSdkVersion 23
	    buildToolsVersion "23.0.0"
	
	    defaultConfig {
	        applicationId "com.one"
	        minSdkVersion 14
	        targetSdkVersion 23
	        versionCode 1
	        versionName "1.0"
	    }
	    productFlavors {
	        xiaomi {}
	        _360 {}
	        anzhi {}
	        wandoujia {}
	    }
	
	    productFlavors.all {
	        flavor -> flavor.manifestPlaceholders = [UMENG_CHANNEL_VALUE: name]
	    }
	
	    signingConfigs {
	        unsigned {
	            storePassword = ""
	            keyAlias = ""
	            keyPassword = ""
	        }
	
	        debug {
	            // No debug config
	        }
	
	        release {
	            storeFile file("../../../Users/ecuser/Desktop/mykey.jks")
	            storePassword "111111"
	            keyAlias "000"
	            keyPassword "111111"
	        }
	    }
	    buildTypes {
	        unsignedBuild {
	            debuggable false
	            versionNameSuffix '-unsigned'
	            signingConfig signingConfigs.unsigned
	        }
	
	        debug {
	            // 显示Log
	            buildConfigField "boolean", "LOG_DEBUG", "true"
	
	            versionNameSuffix "-debug"
	            minifyEnabled false
	            zipAlignEnabled true
	            shrinkResources false
	            signingConfig signingConfigs.debug
	        }
	        release {
	            // 不显示Log
	            buildConfigField "boolean", "LOG_DEBUG", "false"
	//
	            minifyEnabled true
	            zipAlignEnabled true
	            debuggable true
	            // 移除无用的resource文件
	            shrinkResources true
	            proguardFiles getDefaultProguardFile('proguard-android.txt'), 'proguard-rules.pro'
	            signingConfig signingConfigs.release
	            applicationVariants.all { variant ->
	                variant.outputs.each { output ->
	                    def outputFile = output.outputFile
	                    if (outputFile != null && outputFile.name.endsWith('.apk')) {
	                        // 输出apk名称为boohee_v1.0_2015-01-15_wandoujia.apk
	                        def fileName = "one_v${defaultConfig.versionName}_${releaseTime()}_${variant.productFlavors[0].name}.apk"
	                        output.outputFile = new File(outputFile.parent, fileName)
	                    }
	                }
	            }
	        }
	    }
	
	    compileOptions {
	        sourceCompatibility JavaVersion.VERSION_1_7
	        targetCompatibility JavaVersion.VERSION_1_7
	    }
	
	}
	
	dependencies {
	    compile 'com.android.support:appcompat-v7:23.0.0'
	    // 网络请求、上传下载
	    compile 'com.squareup.okhttp:okhttp:2.5.0'
	    compile 'com.squareup.okio:okio:1.6.0'
	    // RecyclerView
	    compile 'com.android.support:recyclerview-v7:23.0.1'
	    compile 'jp.wasabeef:recyclerview-animators:1.3.0'
	    compile 'com.marshalchen.ultimaterecyclerview:library:0.3.17'
	    compile 'in.srain.cube:ultra-ptr:1.0.11'
	    compile 'org.jbundle.util.osgi.wrapped:org.jbundle.util.osgi.wrapped.org.apache.http.client:4.1.2'
	    // textview
	    compile 'me.drakeet.labelview:labelview:0.7'
	    // androidEventbus
	    compile 'org.simple:androideventbus:latest'
	    // RxJava android
	    compile 'io.reactivex:rxandroid:1.0.1'
	    compile 'io.reactivex:rxjava:1.0.15'
	    compile 'com.jakewharton.rxbinding:rxbinding:0.2.0'
	    // retrofit
	    compile 'com.squareup.retrofit:retrofit:1.9.0'
	
	}
	apply plugin: 'me.tatarka.retrolambda'

##4.导出数据库文件

###已有root权限

1.修改权限

	adb shell
	su
	chmod 777 /data
	chmod 777 /data/data
	chmod --recursive 777 /data/data/+packageName

2.拷贝文件
	
	adb pull data/data/packageName/ c:/1

###没有root权限

1.拷贝文件到外部存储
	
	String packageName = getApplicationContext().getPackageName();
    File sdDir = Environment.getExternalStorageDirectory();
    String targetPath = "/data/data/"+packageName+File.separator;
    String toPath = sdDir+File.separator+packageName+File.separator;
    System.out.println("target:"+targetPath+"--"+"topath:"+toPath);
    File file = new File("/data/data/"+packageName);
    if(file.exists()){
        FileUtil.copy(targetPath,toPath);
    }

---
	public class FileUtil {
	public static int copy(String fromFile, String toFile) {
    //要复制的文件目录
    File[] currentFiles;
    File root = new File(fromFile);
    //如同判断SD卡是否存在或者文件是否存在
    //如果不存在则 return出去
    if (!root.exists()) {
        return -1;
    }
    //如果存在则获取当前目录下的全部文件 填充数组
    currentFiles = root.listFiles();

    //目标目录
    File targetDir = new File(toFile);
    //创建目录
    if (!targetDir.exists()) {
        targetDir.mkdirs();
    }
    //遍历要复制该目录下的全部文件
    for (int i = 0; i < currentFiles.length; i++) {
        if (currentFiles[i].isDirectory())//如果当前项为子目录 进行递归
        {
            copy(currentFiles[i].getPath() + "/", toFile + currentFiles[i].getName() + "/");

        } else//如果当前项为文件则进行文件拷贝
        {
            copySdcardFile(currentFiles[i].getPath(), toFile + currentFiles[i].getName());
        }
    }
    return 0;
	}


    //文件拷贝
    //要复制的目录下的所有非子目录(文件夹)文件拷贝
    public static int copySdcardFile(String fromFile, String toFile) {

        try {
            InputStream fosfrom = new FileInputStream(fromFile);
            OutputStream fosto = new FileOutputStream(toFile);
            byte bt[] = new byte[1024];
            int c;
            while ((c = fosfrom.read(bt)) > 0) {
                fosto.write(bt, 0, c);
            }
            fosfrom.close();
            fosto.close();
            return 0;

        } catch (Exception ex) {
            return -1;
        }
    }
	}		


2.拷贝文件
	
	adb pull /storage/emulated/legacy/com.one/ c:/2/2

