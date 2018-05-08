# TFLite bug test
Project to demonstrate TFLite bug where we get a memory addressing error on some armv7 devices.

## Bug description

On certain armv7 devices, we get memory alignment errors. For example, on Galaxy
S3 we see:

```
D/CrashAnrDetector(  351): Build: samsung/d2uc/d2att:4.3/JSS15J/I747UCUEMJB:user/release-keys
D/CrashAnrDetector(  351): Hardware: MSM8960
D/CrashAnrDetector(  351): Revision: 16
D/CrashAnrDetector(  351): Bootloader: I747UCUEMJB
D/CrashAnrDetector(  351): Radio: unknown
D/CrashAnrDetector(  351): Kernel: Linux version 3.0.31-2024954 (se.infra@R0210-11) (gcc version 4.7 (GCC) ) #1 SMP PREEMPT Thu Oct 31 01:05:53 KST 2013
D/CrashAnrDetector(  351):
D/CrashAnrDetector(  351): *** *** *** *** *** *** *** *** *** *** *** *** *** *** *** ***
D/CrashAnrDetector(  351): Build fingerprint: 'samsung/d2uc/d2att:4.3/JSS15J/I747UCUEMJB:user/release-keys'
D/CrashAnrDetector(  351): Revision: '16'
D/CrashAnrDetector(  351): pid: 6048, tid: 6067, name: roidJUnitRunner  >>> com.example.tsob.tflite_bug_test <<<
D/CrashAnrDetector(  351): signal 7 (SIGBUS), code 128 (SI_KERNEL), fault addr 00000000
D/CrashAnrDetector(  351):     r0 5f7d6bd8  r1 00000004  r2 5f7f0250  r3 00000080
D/CrashAnrDetector(  351):     r4 5f6ab6e4  r5 00000000  r6 5f7d6bd8  r7 5f540888
D/CrashAnrDetector(  351):     r8 00000001  r9 5f7f0240  sl 00000000  fp 5f7f0040
D/CrashAnrDetector(  351):     ip 5f7f0040  sp 5f540830  lr 00000080  pc 5f743394  cpsr 00000030
D/CrashAnrDetector(  351):     d0  0000000000000000  d1  4082819d00000000
D/CrashAnrDetector(  351):     d2  000000004147b36c  d3  0000000041476116
D/CrashAnrDetector(  351):     d4  4113563800000000  d5  4114b6d100000000
D/CrashAnrDetector(  351):     d6  0000000000000000  d7  000000003fe5492c
D/CrashAnrDetector(  351):     d8  0000000000000000  d9  0000000000000000
D/CrashAnrDetector(  351):     d10 0000000000000000  d11 0000000000000000
D/CrashAnrDetector(  351):     d12 0000000000000000  d13 0000000000000000
D/CrashAnrDetector(  351):     d14 0000000000000000  d15 0000000000000000
D/CrashAnrDetector(  351):     d16 0000000000000000  d17 0000000000000000
D/CrashAnrDetector(  351):     d18 c0e0bc0ec1100562  d19 c167ff6a40ab03aa
D/CrashAnrDetector(  351):     d20 c07bb5aac01af9fd  d21 c0e3bae1c0916f36
D/CrashAnrDetector(  351):     d22 40bb1fa74085e307  d23 41476116c139dbc3
D/CrashAnrDetector(  351):     d24 3fc74721cad6b0ed  d25 3fc2f112df3e5244
D/CrashAnrDetector(  351):     d26 40026bb1bbb55516  d27 4000000000000000
D/CrashAnrDetector(  351):     d28 40008df2d49d41f1  d29 3fb0f4a31edab38b
D/CrashAnrDetector(  351):     d30 3ff0000000000000  d31 3f4de16b9c24a98f
D/CrashAnrDetector(  351):     scr 80000010
D/CrashAnrDetector(  351):
```

On Galaxy Nexus, we have:

```
F/libc    ( 4638): Fatal signal 7 (SIGBUS) at 0x00000000 (code=128), thread 4651 (roidJUnitRunner)
I/DEBUG   (  124): *** *** *** *** *** *** *** *** *** *** *** *** *** *** *** ***
I/DEBUG   (  124): Build fingerprint: 'google/mysid/toro:4.2.2/JDQ39/573038:user/release-keys'
I/DEBUG   (  124): Revision: '10'
I/DEBUG   (  124): pid: 4638, tid: 4651, name: roidJUnitRunner  >>> com.example.tsob.tflite_bug_test <<<
I/DEBUG   (  124): signal 7 (SIGBUS), code 128 (?), fault addr 00000000
I/DEBUG   (  124):     r0 5d2c6f88  r1 00000004  r2 5a01c250  r3 00000080
I/DEBUG   (  124):     r4 59e866d4  r5 00000000  r6 5d2c6f88  r7 5e61d990
I/DEBUG   (  124):     r8 00000001  r9 5a01c240  sl 00000000  fp 5a01c040
I/DEBUG   (  124):     ip 5a01c040  sp 5e61d938  lr 00000080  pc 5c97f394  cpsr 00000030
I/DEBUG   (  124):     d0  6620746f00000000  d1  732e736b726f7775
I/DEBUG   (  124):     d2  7262696c203a296e  d3  62696c2220797261
I/DEBUG   (  124):     d4  c07bb5aac01af9fd  d5  c0e3bae1c0916f36
I/DEBUG   (  124):     d6  40bb1fa74085e307  d7  41476116c139dbc3
I/DEBUG   (  124):     d8  0000000000000000  d9  0000000000000000
I/DEBUG   (  124):     d10 0000000000000000  d11 0000000000000000
I/DEBUG   (  124):     d12 0000000000000000  d13 0000000000000000
I/DEBUG   (  124):     d14 0000000000000000  d15 0000000000000000
I/DEBUG   (  124):     d16 0000000000000000  d17 0000000000000000
I/DEBUG   (  124):     d18 ff7fffffff7fffff  d19 ff7fffffff7fffff
I/DEBUG   (  124):     d20 41007c5e41512bac  d21 0000000040a5953d
I/DEBUG   (  124):     d22 41007c5e41512bac  d23 0000000040a5953d
I/DEBUG   (  124):     d24 0000000000000000  d25 0000000000000000
I/DEBUG   (  124):     d26 0000002f0000002f  d27 0000002f0000002f
I/DEBUG   (  124):     d28 0000000000000005  d29 0001000000010000
I/DEBUG   (  124):     d30 000000000000000a  d31 000000000000000e
I/DEBUG   (  124):     scr 80000090
I/DEBUG   (  124):
```

However, we do not see errors on the Galaxy S4.

## Example project description

This example project includes a `.tflite` model,
`app/src/androidTest/assets/testmodel.tflite`. This model is loaded via the
instrument test,
`app/src/androidTest/java/com/example/tsob/tflite_bug_test/ExampleInstrumentedTest.java`,
which then runs a hardcoded float input array through the model many times. That
loop of `ExampleInstrumentTest.java` is:

```
        for (int i = 0; i < 1000; ++i) {
            scoreOutput[0][0] = 0.f;
            tflite.run(peaks, scoreOutput);
            Log.i("TFLite Result", "Value: " + scoreOutput[0][0]);

            assertThat(scoreOutput[0][0], greaterThan(-1.0f));
        }
```

As our test model runs through a sigmoid nonliearity at the output, this test
_should_ always return true. Logcat should show something like
`I/TFLite Result: Value: 0.76265603`.

### To run the test

In Android Studio, setup a simple test config,
![alt text](img/test_config.png)
and run it.

## What TFLite version is this?

As specified in `app/build.gradle`, we are using

```
implementation('org.tensorflow:tensorflow-lite:0.1.7@aar')
```
