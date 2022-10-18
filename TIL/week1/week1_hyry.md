🌿VM과 Container의 차이

Virtualization

VM은 hardware를 추상화해 여러 OS를 돌릴 수 있도록 도와준다.

Virtualbox를 사용할 때, 해당 VM에서 cpu, memory는 어느 정도 사용할 건지 지정했는데, Docker를 사용할 때는 그러지는 않았다. 이후 virtual box에 Ubuntu를 직접 깔아줬다.  

Container(container engine)는 OS를 추상화해 여러 application을 돌릴 수 있도록 도와준다.

container에서 다른 OS를 사용하는 경우 base image로 사용하겠다는 한 줄만 Dockerfile에 작성하면 된다. Ubuntu 이미지가 시간이 꽤 걸렸던 것으로 기억한다. Container에서 현재 OS를 그대로 사용하는 경우에는 OS image가 필수가 아니다.  

VM에서는 guest OS 레이어가 별도로 존재

resources를 더 사용하게 된다.

OS를 부팅해야 하기 때문에 시작하는 시간이 오래 걸림

Container는 light-weighted, 즉 VM보다 경량화됐다는 점이 강점

VM이 각각 guest OS가 별도로 존재한다면, container는 container 간에 하나의 host OS를 공유한다.

VM은 보통 GB 단위이고 container image는 MB 단위이다.

Container는 VM보다 리소스를 적게 요구하기 때문에, 더 많은 application을 돌릴 수 있다.

논리적으로 VM이 container보다 확실하게 분리되어 있기 때문에, 보안성이 높다.

🌿Virtual Machine (VM)

software-based computer: Physical한 하나의 컴퓨터(Host machine, physical server) 위에 software 기반, 즉 virtual한 컴퓨터를 여러 대 만들어 사용하는 기술

VM은 physical hardware(물리적 자원)의 virtualization으로 Host에서 여러 OS가 작동하도록 한다.

즉, VM은 하나의 Hardware resources(CPU, memory, Disk)를 공유한다.

구조:  Hardware Infrastructure → (HOST OS layer) → Hypervisor → VM(guest OS + bin/libs + App) 

​

VM끼리는 서로 독립적이다. 하나의 Host 위에 여러 VM이 설치 가능하다.

독립적이기 때문에, VM마다 Windows, Linux, Unix 등 다른 OS도 설치 가능하다.

portable하기 때문에 다른 Host로 VM을 이동하는 것도 가능하다.

​

Hypervisor(VMM; virtual machine manager)

Host machine과 VM 사이에서, Hardware resources를 각각의 VM에 할당하고, 각 VM을 관리하는 소프트웨어

Type1과 Type2 두가지 종류 존재

Type1 (BM; Bare metal hypervisors) : physical server 위에 바로 설치되는 hypervisor

(e.g.) VMware ESXi​​, Microsoft Hyper-V, 오픈 소스 KVM

VM에 설치한 OS (guest OS)가 하드웨어(CPU, memory, disk) 바로 위에서 작동

여러 하드웨어 드라이버를 설치해야 하기 때문에 어려운 설치 과정

Type2 (Hosted) : physical server와 hypervisor 사이에 host OS layer가 존재

(e.g.) Oracle, VirtualBox, VMware Workstation

Type1보다는 쉬운 설치

OS레이어가 하나 더 존재해서 Type1보다 성능이 떨어진다.

 

직접 새 컴퓨터를 사는 것보다는 비용 감소

유지 비용 감소

새 환경 설치하는 것보다는 빠르고 쉽다.

Host가 모종의 이유로 죽었을 때, 다른 host로 이동 가능

🌿Container와 Docker

Containers are lightweight packages of your application code together with dependencies such as specific versions of programming language runtimes and libraries required to run your software services.

Google Cloud

A container is a standard unit of software that packages up code and all its dependencies so the application runs quickly and reliably from one computing environment to another.

Docker

Container: 애플리케이션의 코드와 코드를 돌리는 데 필요한 dependencies를 한꺼번에 담은 패키지

대표적인 컨테이너 기술: Rocket, Docker

구조:  Hardware Infrastructure → HOST OS → Container Engine → VM(guest OS + bin/libs + App) 

Container(container engine)는 OS의 virtualization

​

container engine(Runtime engine)의 예시: Docker engine → 즉, Docker

VM과 마찬가지로 각 애플리케이션을 독립적인 환경에서 구동가능하도록 한다.

각각의 container는 host OS를 공유한다. 

​

OS image를 사용하는 것은 OS를 설치해 사용하는 것과 다르다.

OS를 설치한다 = kernel + filesystem/libraries

image를 사용한다 = filesystem/libraries; 즉, kernel은 Host OS를 이용

container 내부에는 host OS와는 별개로 독자적인 파일 시스템이 존재해서 가능한듯

container는 union file system(a filesystem service for Linux, FreeBSD and NetBS)

This is why you can run only Linux based distribution/binaries within the container. If you want to run something else, it is not impossible, but you would need some kind of virtualization within the container (qemu, kvm, etc.)

​

​

​

​

참고 사이트

[IBM Technology - Virtualization Explained](https://youtu.be/FZR0rG3HKIk)

[IBM Technology - Containerization Explained](https://youtu.be/0qotVMX-J5s)

[TmaxA&C 구르미 - 티맥스(Tmax) 클라우드 온라인 세미나 (2) : VM과 Container의 장단점](https://youtu.be/gw_Lv4SzaH0?list=RDLVgw_Lv4SzaH0)

[Docker - Use containers to Build, Share and Run your applications](https://www.docker.com/resources/what-container/)

[Run Different Linux OS in Docker Container?](https://stackoverflow.com/questions/33112137/run-different-linux-os-in-docker-container​)

[What is the relationship between the docker host OS and the container base image OS?](https://stackoverflow.com/questions/18786209/what-is-the-relationship-between-the-docker-host-os-and-the-container-base-image)

​

​---


🌿Native app & Hybrid app & Cross-Platform app

모바일 애플리케이션의 구분

​

Native app: 특정 모바일 플랫폼 (Android, iOS)를 타깃으로 하는 앱

iOS 디바이스에서 돌아가는 앱이 Android 디바이스에서는 돌아가지 않는다.

특정 OS에 최적화된 퍼포먼스, 네이티브 API 완전히 사용 가능

단점: iOS와 Android, Web마다 다르게 개발 필요하기 때문에 시간과 비용이 많이 든다.

단점: 업데이트를 해도 마켓에 올라가기까지 시간이 걸린다.

Android - Android Studio 이용 - Java, Kotlin 사용

iOS - XCode 이용 - Objective-C, Swift 이용

​

Web-based app (Hybrid App): 웹 표준 기술(HTML5, CSS3, JavaScript)로 구현된 앱

모바일 웹을 앱의 Webview로 제작

(e.g.) APACHE CORDOVA

웹을 한 번 감싸서 native api를 사용할 수 있음

native 지식이 없어도 개발이 가능하다.

웹 하나만 개발하면 웹과 앱 모두 가능

업데이트할 때마다 마켓에 올릴 필요가 없다.

단점: 특정 행동이나 기능에 있어서 internet access가 필요

단점: 앱의 규모와 서비스가 커질수록 native app으로 만들 때보다는 느리게 돌아간다.

단점: 화면을 계속 다시 로드해 UI까지 가져와야 하기 때문에, 데이터만 가져오면 되는 native app보다는 데이터 소모량이 크다.

​

Cross-Platform app: native와 web-based app이 섞인 개념

코드를 작성하면 Native가 이해할 수 있는 코드로 compile

Flutter, React Native,​​ Xamarin 등이 속한다. 

웹과 네이티브를 걸쳐 여러 플랫폼에서 호환

하이브리드에 비해 상대적으로 속도가 빠르고 성능이 좋다.

개발이 쉽고 빠르다는 장점

단점: 네이티브의 모든 기능을 제공하지는 않으며, 네이티브에 신 기술이 업데이트되더라도 크로스 플랫폼이 업데이트하기 전까지 시간이 걸린다.

단점: 사업이 중단되어 프레임 워크가 사라질 가능성이 존재

​

​

​

참고 사이트

[Wikipedia - Mobile app](https://en.wikipedia.org/wiki/Mobile_app#Native_app)

​

​