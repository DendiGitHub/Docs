# JNI #
# JaveNativeInterface #

## Reason ##
使用JNI是有风险的，会使得Java程序失去跨平台性。同时本地代码的不当使用会使整个java程序崩溃。

## JVM及其他 ##
在JNI中，jvm是一个全局的指针。Jvm中的每一个线程有一个JNIEnv指针，该指针是线程相关的，在同一个线程中获取到的JNIEnv是同一个指针。

利用JNIEnv指针我们可以获得在native中创建的全局变量

## 编写方法 ##

- 编写Java代码，方法修饰符加上native
- javac XX.java生成.class文件
- javah 生成.h文件
- 编写本地实现代码


## Java调用C层代码 ##

C的方法名为Java_class_method(JNIENV *pEnv,jObject obj,params...)

    //Java
    public native int scanDevicesNative(long pointer);

    //C
    jint Java_com_tplink_ipc_core_IPCDeviceListContext_scanDevicesNative(JNIEnv * pEnv, jobject obj, jlong pointer) {
        //找到class
        jclass ClzIPCDeviceListGlobalRef = pEnv->FindClass("com/tplink/ipc/core/IPCDeviceListContext");
        if (ClzIPCDeviceListGlobalRef == NULL) {
            return -1;
        }
        ClzIPCDeviceListGlobalRef = (jclass)pEnv->NewGlobalRef(ClzIPCDeviceListGlobalRef);

        //找到MethodID，三个参数分别为jclass,methodName,signature
        jmethodID addSearchedDeviceMethodID = pEnv->GetMethodID (ClzIPCDeviceListGlobalRef,"addToSearchedDeviceList","(Ljava/lang/String;Ljava/lang/String;J)V");


        for (int i = 0; i < iCount; i++) {
            pEnv->CallVoidMethod(ClzIPCDeviceListGlobalRef,addSearchedDeviceMethodID, char2Jstring(pEnv, pScannedNewDevices[i].pcIP),
                             char2Jstring(pEnv, pScannedNewDevices[i].pcMAC), pScannedNewDevices[i].ullID);
        }

        return 0;
    }

## C调用Java代码 ##

    //找到class
    jclass ClzGlobalRef = pEnv->FindClass("com/tplink/ipc/core/IPCDeviceListContext");
    if (ClzGlobalRef == NULL) {
        return -1;
    }
    ClzGlobalRef = (jclass)pEnv->NewGlobalRef(ClzGlobalRef);

    //找到MethodID，三个参数分别为jclass,methodName,signature
    jmethodID jMethodID = 
        pEnv->GetMethodID (ClzGlobalRef,
            "ThisIsTheMethodName",
                "(Ljava/lang/String;Ljava/lang/String;J)V");

    jobject ObjGlobalRef = pEnv->NewGlobalRef(obj);

        pEnv->CallVoidMethod(ObjGlobalRef,jMethodID,
            char2Jstring(pEnv,pScannedNewDevices[i].pcIP),
            char2Jstring(pEnv, pScannedNewDevices[i].pcMAC), 
            pScannedNewDevices[i].ullID);

### Signature ###

| Type Sinature        | Java Type           |
| ------------- |:-------------:|
| Z      | boolean |
| B     | byte      |
| C | char     |
| S | short     |
| I | int      |
| J | long      |
| F | float      |
| D | double      |
| Lfully-qialified-class | fully-qualified-class   |
| [type | type[]   |
| (arg-types)ret-type | method type     |


for example ,the Java method:

    long f(int n,String s,int[] arr);

has the following signature:
    
    (ILjava/lang/string;[I)J

## 注意 ##

jint对应的是CCPP中的long类型

## 一些方法 ##

    //取得jclass对象
	jClass cls_strnig = pEnv -> FindClass("java/lang/Strnig")

	jfieldID GetFieldID(jclass clazz, const char *name,const char *sig)

	jfieldID GetStaticFieldID(jclass clazz, const char*name, const char *sig)

	jmethodID GetStaticMethodID(jclass clazz, const char*name, const char *sig)

	jmethodID GetMethodID(jclass clazz, const char *name,constchar *sig)

	//调用方法
	Call<Type>Method(jobject obj, jmethodID methodID,...);

	CallStatic<Type>Method(jclass clazz, jmethodID methodID,...);