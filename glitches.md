When completed as required, the output of the running program will occasionally contain 'glitches' where one tasks output stomps on another tasks output. 

<code>
TaskThree: count = 415
TaskThree: count = 416askOne: Turn LED ON

TaskThree: count = 417


TaskThree: count = 514
TaskThree: coaskOne: Turn LED ON
unt = 515
TaskThree: count = 516
</code>

This can be remedied by using a mutex, and some change to interrupt.c 

Some of the code needed for this change is as follows:

<code>
void GetMutex() 
{
    uint32_t mutex_acquired = 0; 
    
    while (!mutex_acquired) {  
        __asm(" CPSID i"); 
        if (mutex == 0) {
            mutex = 1; 
            mutex_acquired = 1; 
        }
        __asm(" CPSIE i");
        
        // just wait if we couldn't get hold of the mutex
        if (!mutex_acquired) 
            OSTimeDly(1);
    }
}


void ClearMutex()
{
    __asm(" CPSID i"); 
    mutex = 0; 
    if (sigint) { 
        sigint = 0; 
        SCB->ICSR |= SCB_ICSR_PENDSVSET_Msk;
    }
    __asm(" CPSIE i");
}


extern volatile uint32_t mutex;
extern volatile uint32_t sigint;    // when set it signals that an interrupt 
                                    // was ignored due to a set mutex
void SysTick_Handler(void)
{
    // switch context by triggering PendSV exception if the nutex is not set
    // if the mutex is set just increment 'sigint', so that the nutex release
    // function can call this SysTick_Handler anew
    __asm(" CPSID i"); 
    if (mutex > 0) {
        sigint++; 
    } else {
        sigint = 0;
        SCB->ICSR |= SCB_ICSR_PENDSVSET_Msk; // Set PendSV to pending
    }
    __asm(" CPSIE i");
}
</code>


