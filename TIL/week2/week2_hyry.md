# 🌿Computer

고유 특징: programmable → 다양한 용도로 활용 가능

핵심 기능: computation

컴퓨터를 구성하는 것들

Processor(s), Memory, Input devices, Output devices

Interconnects: buses, Motherboards, chipsets, ...


🌿Processor

Processor = CPU: central processing unit

memory에서 Processor 내부의 register로 instruction을 하나씩 가지고 온다.

instruction 실행

무슨 operation을 해야하는지 decode

어떤 것들을 읽어와야 하는지, 어떤 연산을 해야하는지, 연산 결과를 어디에 저장해야 하는지, 이후 결과를 update

데이터를 memory에서 가져오고 보낸다.

(e.g.) Quad-core: CPU 4개를 보유

각각의 CPU마다 명령을 개별적으로 가져올 수 있다 → 매 번 4개의 명령이 동시에 실행

(e.g.) Intel I7 Processor, Smartphone processor는 ARM(생산은 Samsung)이나 Apple 회사에서 설계

x86 (IA32; intel architecture 32 → IA 64), ARM, MIF, SPARC, AMD64(X86에 데이터 크기만 64로; X86-64)

Architecture

대부분의 컴퓨터는 x86 사용: X86 Family

32-bit processor (연산 기본 단위가 32 bits)

e.g. 386, 486, Pentium → 내부 설계가 다르다 (generation)

스마트폰은 ARM 많이 사용

machine language

동일한 명령어 집합 구조



🌿Memory

프로그램 (instruction + data) 저장

휘발성: Caches, main memory

SRAM (static RAM): 전원이 공급되는 한 동일한 상태 유지 가능 → caches

DRAM (dynamic RAM): main memory에만 사용된다.

비휘발성: HDD, CD-ROM, DVD, ROM, FLASH, ...
