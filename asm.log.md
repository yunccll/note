1. 指令处理
    指令指针 处理器跟踪代码位置                     Low->High Upward    (InstructionPointer)
    数据指针 处理器跟踪数据域在内存中的开始位置　　 High->Low Downward  (StackPointer)

    图：　Low 在下面，　High 在上面　作为参考系

2. 指令格式

    [0-4B Prefix] 1-3B OpCode [ 0-6B Modifiers] [0-4B Data Element]

    Modifiers:
        0-1B Mod R/M, 0-1B SIB, 0-4B Displacement
    
    Prefix: 四组策略，每组策略同时只能有一个,
        1. lock and repeat  (lock -> share data; repeat usually for strings handle)
        2. segment override & brand hint  
        3. operand size override
        4. address size override
    OpCode:  必填,　表明处理器的基本功能,数据　要么在寄存器，要么在内存
    Modifiers:
        Mod R/M: 定义寄存器和内存参与该指令
        SID: scale-index-base 　操作的缩放因子，指示索引寄存器(si,di)，指示ｂａｓｅ寄存器(bp)
        Displacement:
    Data:

3. directives 指示　(.XXX)
    .section (data, bss, text)
    .long  .ascii .float .asciz



4. IA32　CPU架构 & 寄存器
    4.1 CPU　与其他部件关系
    processor  ----  system memory,  input devices,   output devices
    --- :  control bus; address bus; data bus

    CPU　主动发送控制指令到 control bus
    CPU 发送地址到 address bus
    CPU 发送和接收Data data bus

    4.2 CPU内部组成
        控制单元，执行单元，寄存器, 标志位

5. 寄存器   
    5.1 分类：
    1. General Purpose:     8个　32bit，EAX, EBX, ECX (counter for loop) ,EDX(IO pointer), EDI(dest index), ESI(source index)  ESP(stack pointer)  EBP(base pointer)
    2. Segment:             6个，16bit, CS, DS, SS, ES, FS, GS  内存的(分段，扁平)模式
    3. Instruction pointer  1个, 32bit, EIP
    4. Float-point data:    8个, 80bit, FP0, FP1, FP2, FP3, FP4, FP5, FP6, FP7
    5. control:             5个, 32bit, CR0, CR1, CR2, CR3, CR4 ( system software manager it)
    6. flags:               1个, 32bit, EFLAGS, 分三组 status flags, control flags, system flags
    7. debug:               8个, 32bit,

    5.2 说明
    1>. control register
        CR0 - System flags that control operating mode & states of processor
        CR1 - not currently used
        CR2 - memory page fault information (the address that visit the page fault memory)
        CR3 - Memory Page Directory information
        CR4 - Flags that enable processor features & test the feature capability

    CRX 只能与 General Purpose Register 进行交互；

    2>. Flags: 
      1) status flags
        CF(Carray) 0bit, PF(Parity) 2bit, AF(Adjust) 4bit, ZF(Zero) 6bit, SF(sign) 7bit, OF(overflow) 11bit
      2) control flags
        DF   0->increment, 1->decrement  
      3) System flags
        TF(Trap) 8bit,  IF( interrupt enable) 9bit, IOPL( i/o privilege leve) 12~13bit
        NT(nested task) 14bit, RF(Resume) 16bit ,VM(virtual-8086 mode) 17bit
        AC(Alignment check) 18bit, VIF(Virtual interrupt) 19bit
        VIP(Virtual interrupt pending) 20bit, ID(Identification) 21bit

6. toolchain - assembler, linker, debugger, compiler, object code disassembler, profiler
    6.1 assembler 
        addr2line, gprof, c++filt (demangle 解析后的完整函数签名), objcopy, objdump, readelf, size,  strings, strip

        gas 选项:
            --gstabs debugging information
            -I    
            -o output.file.name
            -R fold the data section into the text section

        -fPIC : big topic TODO:
            位置独立变异， 与重定位有关； 重定位源于 shared lib 机制  和  object file link process

            shared-lib : 在 具体 process 中的map address 不同； 因此，必须进行重定向。
            重定位对象： global data & text  in ( 自己调用， 外部process 调用 shared library)
            分为两个阶段：  link-time    load-time 重定位 (libdl.so)

            
    6.2 linker
        ld 选项:
            -b 
            -Bstatic 
            -Bdynamic
            -BSymbolic
            -demangle   demangle symbol names in error messages
            -e          entry 
            -format -b  binary
            -shared     shared library
            -Ttext      text 段开始地址
            -Tdata      data 
            -Tbss       bss
            -t          显示处理中的输入文件名
            

    6.3 debugger  gdb 
      gdb  -c core  -e executable -d src_code_search_directory

      break b 
      watch w
      info  
    * x     x   examine memory location, e.g. x/-100xw  addr   往上100个word， 以hex(x)打印。具体查看help x
    * print p   display variable values,   $rax, $eax, $ax, $al
      run   r
      list  l
      next  n
      step  s
      cont      continue executing the program 
    * util      run the program util it reaches the specified src code line  *******


    6.4  objdump   ,  readelf , objcopy 
        objdump  -S -d  disassembly   
      * -S  mix the src code with asm
      * -p  display file header contents
        -h  contents of the section headers
        -T  dynamic symbol table 
        -r  relocation entries 
        -R  dynamic relocation entries 
      * -C  demangle symbol to readable

      readelf  -h  head 

      objcopy 
        

7. opcode syntax 
    gas => AT&T syntax ; 这是当时UNIX使用最流行处理器所使用的语法; intel没有选择这种语法
    intel vs AT&T 
    立即数:     AT&T $,  Intel 无
    寄存器:     AT&T %,  Intel 无
    操作方向:   AT&T src->dest,  Intel   dest <- src
    数据长度:   AT&T b/w/l/q,  Intel    mov eax, dword ptr test
    跳转:       AT&T ljmp $section, $offset,  Intel jmp section:offset


