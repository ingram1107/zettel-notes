---
title: "A Review on Android Malware: Attacks, Countermeassures and Challenges"
authors: [Shymala Gowri Selvaganapathy, Sudha Sadasivam, Vinayakumar Ravi]
year: 2021
date: 2022-10-29 10:41
tags: [literature, security]
---

This article summarises the possible attacks and current defences in Android
space in the realm of malware.

# Malware Categorisation

Malware could be categorised according to their propagation procedures and their
activities accomplished on the infected system. There are two main types of
malware: targeted attacks and untargeted attacks. Targeted attacks are usually
Advanced Persistent Threat (APT), whereas the untargeted attacks (mass attacks)
could be further categorised based on their functionalities. If it is
infectious, then it is either a *virus* or *worm*. If it is concealed within a
single system, then it could be *Trojan Horse*, *backdoor*, *logic bomb* or
*rootkit*. If it steals information, then it could be *keyloggers*, *spyware* or
*adware*. If its goal is to make a profit, then it can be as *scareware*,
*ransomware*, *crypto-mining malware* or *spam sending malware*. If it can be
controlled by command and control server, then it is a *botnet*.

It could be in various form such as executable file format, non-executable file
format or [fileless](../202211181709.md)#.

Mass malware are less sophisticated and can be defended easily.

APT will try to remain undetected for a long period and later causes substantial
damage to the specific target.

Infectious malware are those which can spread to infect new victims, either
through disks, e-mail, messaging devices, [Internet Protocol (IP)](../202206151223.md)#,
compromised legitimate websites, advertisements and so on.

Virus is one of the infectious malware, which is defined as a code segment which
forces a host program to execute its logic. Basically, it injects malicious
contents into a legitimate program and infects it. The virus take control of the
program once it is executed, and executes its payload. Examples are Crossover,
Dust, Lasco, and CardTrap.

Worm is another infectious malware, which is defined as an unallied and
self-reliant program which propagates and replicates by spawning itself on
infested nodes and copies itself on other nodes if there is a connection
facility established. Examples are Cabir, ZeuS MitMo, Beselo and CommWarrior.

Concealment malware are those that try to hide its detection. Persistence
mechanism is employed to do so.

Trojan horse is a concealment malware which disguise its malicious payloads by
advertising it as a legitimate entity. It doesn't need for a host program to
work, and it doesn't spread. The examples of trojan horse including DownAPK,
GatSPY, MasterKey, Fontal, Liberty Crack and Infojack.

Backdoor, or sometimes called trapdoor, is a concealment malware which stealthy
gain access to entities (system or program) in a fabricated way that is not
supported by their designers. It is usually the payload of other malware such as
worm and trojan horse. Examples of backdoor are: Brador (aimed at Windows mobile
OS) and Twitoor (aimed at Android).

Logic bomb is a delayed-action concealment malware which will execute its code
segment once condition(s) is satisfied. For example, RCSAndroid waits for SMS
messages from certain contacts, or Holy Colbert waits for the specific date.

Rootkits are another concealment malware which are a set of software tools
usually used by a malicious actor to gain and maintain privileged access to a
compromised system in an unauthorised manner. It remained undetected with
sophisticated techniques to alter system file. The host that has rootkits
installed in it is called a rooted host.

There are malware designed around stealing information from the infected hosts
by recording the keystrokes (keyloggers), inspecting user activities in the
system (spyware), or collect and transmit user preferences (adware). They are
aiming to steal critical information such as banking credentials, login details,
identifiable information etc. Examples of keyloggers are Mysterybot (predicting
the key pressed by collecting user's touch gesture screen position) and Mspy.
Spyware is usually more hidden, one of the instances is Flexispy. Adware are
more steer towards their marketing purposes. Few examples are the Car Racing
2019 and Mobnet.io Screen Stream Mirroring.

Scareware is one of the malware that aimed for profiting by inducing fear to its
user in order to convince them into purchasing needless software. One of the
example is AndroidDefender which raise fake alerts and insists its user to buy
their product to protect their device.

Ransomeware is also another profit driven malware that hold file contents or
system as hostage by encryption or access locking. The attacker will ask for a
ransom in order to release the corresponding locked computer resources while
some would threaten a disclosure of the data such as those in Maze and
Sodinokibimb families of ransomeware. Other examples are Filecoder, WannaLocker,
LeakerLocker and Xbot.

Spam-sending malware utilises the infected machine to send spam messages in
order to generate revenues for the attacker.

Crypto-mining or cryptojacking malware is a new form of profit centric malware
that loots the infected machine's computer resources with the purpose of
performing mining operations on cryptocurrencies. It typically conceals itself
from being detected by the victim.

Botnet is a form of malware where the attacker can access and operate on the
infected machine, with addition that it will listen the instructions from a
command and a control server. The compromised machines usually refered to as
zombies. A common use of botnet is to launch attacks via zombies to carry out
[Denial of Service (DoS)](../202209262115.md)#, spam contents spreading and/or
performing click frauds. Examples of botnet are Geost, Chaois, and Twitoor.

There is an emerging form of malware in Windows that is
[fileless](../202211181709.md)# known as Advanced Volatile Threats (AVTs). Such
malware will operate in the volatile main memory which could avoid detection
from anti-analysis techniques. Possible attack vectors including PowerShell,
[Windows Management Instrumentation (WMI)](../202211211002.md)#, command prompt,
.NET framework, and Remote Desktop Protocol (RDP). AVTs utilise scripts such as
JavaScript and Visual Basic, and compile HTML files under the hood of a system
process.

# Detecting a Malware

Determine whether an executable file is a malware could be done through two
methods: malware analysis and malware detection. Antivirus can detect a malware
with either of these mechanisms or a combination of both.

Malware analysis could be static, dynamic or hybrid (a combination of static and
dynamic analysis). Static analysis aka structural analysis or code analysis
involving dividing the code into smaller pieces and inspect the code without
actually executing it. However, it could be obfuscated by the attacker with
encryption, compression, dynamic linking, dead code insertion, polymorphism etc.
Dynamic analysis or behavioural analysis technique inspects the runtime
behaviour of the executable in a constrained environment (sandbox or emulator)
for a limited period. It has a drawback of not being able to detect all possible
threats and still vulnerable to system-call obfuscation techniques and
anti-analysis techniques.

Malware detection could be signature based or anomaly based. Signature based
malware detection utilises a repository of well-known malware signatures and
compare it to the incoming samples. Obviously, if the repository doesn't keep up
to date, it will leave the system vulnerable to threats that is not known to the
anti-malware engine. Anomaly based malware detection compares the executable's
behavioural patterns with normal patterns and try to raise an alert when there
are any deviations found. Although it is quite useful in detecting unknown
malware and zero-day attacks, it is prone to false positive. It can deploy
expert systems, string matching, state modelling, rule-based systems, genetic
algorithms, immune system models and learning based technique in order to reduce
false positives.

# Structure of An APK File

Android Application Package aka APK file is a package format for Android
applications. It is digitally signed with the application developer's
certificate which consists of *AndroidManifest.xml* (holds permissions,
application version, and libraries used by the application), *LIB* folder
(contains native libraries which will be loaded by the Java Native Interface,
JNI at runtime), *META-INF folder* (contains signature files CERT.RSA, CERT.SF,
and MANIFEST.MF), *resources.arsc* (contains the precompiled compressed
resources), *RES folder* (contains resources which have an ID assigned during
compilation), and *ASSETS folder* (contains application assets such as media
file etc, which are accessible through the Asset-Manager class).

# Inspecting An APK File

To have a competent malware detection, benign samples and malicious samples of
Android applications should be collected in a balance manner.

In static analysis, APK is unpacked, decompiled, and inspected for malicious
contents without executing it. Static features are extracted from
Android-Manifest.xml and DEX code file. Example of tools that could do this are
Androguard, apktool, dex2jar, radare2, dexter, and IDAPro. RemNux and Android
Reverse Engineering are useful too for malware analysis. To explore obfuscation
techniques, ProGuard, DexGuard and DexProtector should be inpected to see how
much an attacker could do to bypass the analysis.

The features that could be extracted are as follows:
- permissions used
- application components such as Service (background execution), Activity (UI),
  Content Provider (data exchange across applications) and Broadcast Provider
  (system-wide announcements)
- filter intents (messages sent between components used by Android message
  passing system)
- API calls
- network addresses
- operation codes, their frequencies, and sequences
- native code
- access of hardware components

Heavyweight static analysis uses various graph based data structures, such as
API Call Graph (CG), Control Flow Graph (CFG), and Data Flow Graph (DFG), to
analysis a structure of a program. API CG depicts a [Directed
Graph](../202204112118.md)# with denotes links between callee and caller using
edge. CFG represents the set of all paths traversed during program execution.
DFG visualises the flow of sensitive information across the application between
different entities. These techniques require heavy use of computer resources.

The lightweight approaches to static analysis in Android platform are
RevealDroid, LightDroid, DroidSevie, Tinydroid, 1D-CNN, and DroidKin. #review

Lightweight approaches to malware detection in Android platform are IntelliAV
(TensorFlow based machine learning), Drebin, and Qualcomm's Snapdragon Smart
Protect (machine learning). BL-AMD is an alternative for online training to the
former examples which all of them are offline training then embedded to Android.

Commonly used ML and DL frameworks for malware detection are Convolutional
Neural Network (CNN, good at finding anomalies in images), Recurrent Neural
Networks (RNN, commonly used in hard-writing recognition and natural language),
and Deep Neural Network (DNN, better generation elation capabilities).
