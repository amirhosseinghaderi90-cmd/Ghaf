```mermaid
flowchart TD
    subgraph University ["دانشگاه / سیستم"]
        U_Start(( )) --> U_Task1["تعریف دروس ارائه شده"]
        U_Task1 --> U_Task2["دسترسی دادن به دانشجویان"]
    end

    subgraph Student ["دانشجو"]
        S_Start(( )) --> S_Task1["ورود به کارتابل انتخاب واحد"]
        U_Task2 --> S_Task1
        S_Task1 --> S_Task2["اضافه و حذف از سبد دروس انتخابی"]
        S_Task2 --> S_Gateway{"ثبت تغییرات"}
        
        S_Gateway -->|موقتی| S_Task3["ثبت موقت"]
        S_Task3 --> S_Task2
        
        S_Gateway -->|نهایی| S_Task4["ثبت نهایی"]
        S_Task4 --> S_Task5["پرداخت"]
        S_Task5 --> S_Task6["چاپ انتخاب واحد"]
        S_Task6 --> S_End([پایان])
    end
