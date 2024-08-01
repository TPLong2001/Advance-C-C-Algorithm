# Lesson 3: Pointer

<details>
  <summary>chi tiết</summary

## 1. Pointer
 Kích thước pointer: kích thước của pointer phụ thuộc vào kiến trúc máy tính thực thi
+ Kiến trúc x86, con trỏ sẽ có kích thước 32-bit (4 bytes)
+ Kiến trúc x64, con trỏ sẽ có kích thước 64-bit (8 bytes)
  
Định nghĩa: Biến con trỏ là một biến có giá trị là địa chỉ của một biết khác ( chứa địa chỉ của một biến khác). Biến bình thường thì chỉ lưu giá trị
    
```c
int main{
    int var = 10;  //bien binh thuong
    int *ptr;      //bien con tro
    ptr = &var;    // bien con tro lay dia chi cua bien var

    //ex1: pointer
    int a = 10;
    int array[]={1,2,4,5};

    int *ptr = array;

    printf("Dia chi a: %p \n", &a);
    printf("gia tri tai dia chi: %d\n",*ptr);
    printf("Dia chi Tong: %p \n", &tong);
    printf("Dia chi array: %p\n",array);
}
```
## 2. function pointer
#### Con trỏ hàm là 1 con trỏ lưu trữ địa chỉ của 1 hàm mà nó đang trỏ đến. Nó cũng có thể truyền một fumctiom như một đối số cho function khác.
```c
#include<stdio.h>
void tong(int a, int b){
    printf("Tong %d va %d = %d\n",a,b,a+b);
}
void hieu(int a, int b){
    printf("hieu %d va %d = %d\n",a,b,a-b);
}
void tich(int a, int b){
    printf("tich %d va %d = %d\n",a,b,a*b);
}
void thuong(int a, int b){
    printf("thuong %d va %d = %f\n",a,b,a/(double)b);
}
void TinhToan(void (*phepToan)(int,int),int a, int b){
    printf("Chuong trinh tinh toan:\n");
    phepToan(a,b);
}
int main (int argc, char const *argv[]){
//ex2: func pointer
    void (*phepToan[])(int,int) = {&tong, &hieu, &tich, &thuong};

    phepToan[0](7,5);
    phepToan[1](7,5);
    phepToan[2](7,5);
    phepToan[3](7,5);

    TinhToan(&tong,27,65);
    TinhToan(&hieu,27,65);
    TinhToan(&thuong,27,65);

    char (*text)() = &display;
    printf("Test: %c\n",text());
    return 0;
}
```


## 3.Type pointer
#### Các kiểu dữ liệu của con trỏ cho thấy con trỏ đang lưu trữ dữ giá trị kiểu giữ liệu nào tại địa chỉ đó.
#### VD: char *ptr; đây là con trỏ kiểu char nó sẽ sử dụng 1 byte địa chỉ tương úng để chứa giá trị. Nếu có các giá trị xếp liên tiếp (như kiểu mảng, kiểu chuỗi) thì xác định theo độ lệch địa chỉ cũng dựa vào kiểu dữ liệu của con trỏ đó, từ đó việc dịch địa chỉ để lấy giá trị trong 1 chuỗi các giá trị liên tiếp mới chính xác.
#### con trỏ kiểu void có thể trỏ đến tất các các con trỏ, địa chỉ khác mà không cần quan tâm đến kiểu dữ liệu của con trỏ, địa chỉ đó.
```c
#include<stdio.h>
int main (int argc, char const *argv[]){
//ex3: type pointer 
    int var_int = 50;
    char var_char = 'A';
    double var_double = 10.5;
    
    int *ptr;
    char *ptr_char;
    float *ptr_float;
    double *ptr_double;

    void *ptr_void;

    ptr_void = &tong;
    ((void (*)(int,int))ptr_void)(3,4);

    ptr_void = &var_char;
    printf("Dia chi: %p, int: %c\n",ptr_void,*(char *)ptr_void);    
    ptr_void = &var_double;
    printf("Dia chi: %p, int: %f\n",ptr_void,*(double *)ptr_void);    
    ptr_void = &var_int;
    printf("Dia chi: %p, int: %d\n",ptr_void,*(int *)ptr_void);     
    return 0;
}
```


## 4.Pass by value và Pass by reference
#### Khi truyền các đối số vào 1 hàm thì:
#### -Nếu truyền int a, int b (Pass by value) thì khi hàm đó được gọi, các đối số truyền vào sẽ được khởi tạo 1 bản sao của nó hoàn toàn không liên quan đến các đối số đã truyền vào lúc đầu, các bản sao đó sẽ được thực thi trong hàm và được xóa khi kết thúc hàm vì vậy giá trị của các đối số lúc truyền vào sẽ không bị thay đổi.
#### - Nếu truyền int *a, int *b (Pass by reference) thì khi hàm đó được gọi, các đối số truyền vào sẽ có giá trị chính là địa chỉ cuả các đối số truyền vào đó, lên khi thực thi trong hàm thay đổi giá trị của biến thông qua địa chỉ sẽ thay đổi hoàn toán giá trị của biến đó, kế cả khi thoát khỏi hàm.
#### - Con trỏ hằng: là 1 biến con trỏ trỏ vào 1 giá trị hằng, vì vậy không thể thay đổi giá trị của hằng đó kể cả thông qua con trỏ, còn ta vẫn có thể sử dụng các con trỏ khác để trỏ vào hằng đó.
#### - Hằng con trỏ: là 1 biền con trỏ kiểu hằng (không thể thay đổi giá trị tại nó được lưu trong bộ nhớ vùng data segment) vì vậy khi dã gán cho nó 1 địa chỉ để trỏ vào thì ta không thể thay đổi địa chỉ trỏ vào đó hay nói cách khác ta không thể trỏ đến giá trị khác nữa. còn giá trị trỏ vào vẫn có thể thay đổi giá trị.
```c
#include<stdio.h>
void swap(int a, int b){    //int a=10, int b=20, 0xc3=10 va 0xc4=20
    int c;
    c = a;
    a = b;
    b = c;
}

void swap2(int *a, int *b){     //int *a=0x01  int *b=0x02
    int c;
    c = *a;
    *a = *b;
    *b = c;
}
int main (int argc, char const *argv[]){
//ex4: tham chieu bien, con tro hang, hang con tro
    int x = 10, y = 20;     //0x01=10 0x02=20
    printf("x=%d, y=%d\n",x,y);

    swap2(&x,&y);
    printf("x=%d, y=%d\n",x,y);

    printf("String: %s\n",string);

    // display2(string);
    // printf("String: %s\n",string);
    return 0;
}
```


## 5.NULL Pointer 
#### là con trỏ trỏ đến vùng nhớ bằng không hay nói cách khác nó không trỏ đến bất kì đối tượng hay vùng nhớ nào. nó được ứng dụng cho việc khai báo các con trỏ ban đầu khi chưa có đối tượng trỏ cụ thể để tránh trỏ vào các vùng nhớ rác không mong muốn gây lỗi hệ thống.


## 6.Pointer to pointer
</details>

# Lesson 4: Goto_setjmp
<details>
  <summary>Chi tiết</summary>

  # 1. goto
  ## Định nghĩa: 
  "goto" là một từ khóa trong C, câu lệnh goto là một câu lệnh nhảy (jump) vô điều kiện. Khi sử dụng câu lệnh nó sẽ nhảy đến một nhãn đã đặt trước đó.
việc nhảy vô điều kiện như vậy khiến ta rất dể dàng sử dụng để kiểm soát flow của chương trình, nhưng nếu lạm dụng nó có thể khiến cho chương trình trở lên khó đọc và bảo trì.
  ### Ví Dụ
 ```c
#include<stdio.h>

int main(int argc,char const *argv[]){

    batdau:
    for (int i = 0; i < 10; i++)
    {

        printf("i = %d\n",i);
        if(i == 5){
            goto batdau;
        }
    }
    return 0;
}
```
Trong ví dụ này sau khi i tiến tới 5 nó sẽ đọc đến câu lệnh goto để nhảy đến nhãn "batdau" và lại bắt đầu vòng lặp từ i = 0, vì vậy vòng lặp này sẽ chạy vô hạn từ 0 đến 5 và quay lại.

  ## Ưu điểm của goto
  ### Tự tạo 1 vòng lặp cơ bản
  goto có thể được sử dụng để tạo một vòng lặp đơn giản bằng cách gắn nhãn nhảy và kiểm tra điều kiện thoát.
  ### Ví Dụ
```c
#include <stdio.h>

int main() {
    int i = 0;

    start:
        if (i >= 5) {
            goto end; 
        }
        printf("%d ", i);
        i++;
        goto start;
    end:
        printf("\n");
    return 0;
}
```
Khi i đạt đến giá trị 5, control sẽ chuyển đến nhãn "end" và kết thúc chương trình.

  ### Thoát khỏi vòng lặp nhiều cấp độ
Nhiều trường hợp có nhiều vòng lặp lồng nhau, khi đó để thoát khỏi toàn bộ hoặc 1 số vòng lặp sẽ rất khó khăn, ta sẽ phải kiểm tra điều kiện thoát vòng lặp liên tục ở các vòng lặp sau đó. Để giải quyết việc này ta có thể sử dụng goto để nhảy đến 1 nhãn đã đặt ở nơi cần thoát ra.
  ### Ví Dụ
```c
for (int i = 0; i < 10; ++i) {
    for (int j = 0; j < 10; ++j) {
        if (some_condition(i, j)) {
            goto exit_loops;
        }
    }
}
exit_loops:
```

  
### Xử lý lỗi và giải phóng bộ nhớ
Trong trường hợp xử lý lỗi, có thể sử dụng goto để dễ dàng giải phóng bộ nhớ đã được cấp phát trước khi thoát khỏi hàm.
### Ví Dụ
```c
void process_data() {
    int *data = malloc(sizeof(int) * 100);
    if (data == NULL) {
        goto cleanup;
    }
    cleanup:
    free(data);
}

```
  ### Implement Finite State Machines (FSM)
Trong một số trường hợp, đặc biệt là khi triển khai Finite State Machines, goto có thể được sử dụng để chuyển đến các trạng thái khác nhau một cách dễ dàng khi thỏa mãn điều kiện đề ra trước đó, ta có thể cho nó nhảy sang xử lý các điều kiện khác.
### Ví Dụ
```c
switch (current_state) {
    case STATE_A:
        // Xử lý State A
        if (condition) {
            goto STATE_B;
        }
        break;
    case STATE_B:
        break;
}

```
   ## Nhược điểm của goto
  + Việc sử dụng câu lệnh goto không được khuyến khích vì nó làm cho logic chương trình trở nên rất phức tạp.
  + Việc sử dụng goto làm cho việc theo dõi dòng chảy của chương trình trở nên rất khó khăn.
  + Việc sử dụng goto làm cho nhiệm vụ phân tích và xác minh tính đúng đắn của các chương trình (đặc biệt là các chương trình liên quan đến vòng lặp) trở nên rất khó khăn.
  + Việc sử dụng goto có thể tránh được một cách đơn giản bằng cách sử dụng câu lệnh break và continue .

# 2. setjmp.h
  ## Định nghĩa: 
  "setjmp.h" là một thư viện trong C cung cấp các hàm để xử lý các ngoại lệ theo mong muốn. có 2 hàm chính là setjmp và longjmp để thực hiện xử lý các ngoại lệ.
```c
setjmp(buf) == 0
longjmp(buf,(x))
```
  ## Ví Dụ:
```c
#include<stdio.h>
#include<setjmp.h>

jmp_buf buf;
int exception_code;

int main(){

    exception_code = setjmp(buf);

    //ex1
    if (exception_code == 2);
    {
        printf("Nhay den 2\n");
        //exit();

    }
    printf("Test: %d\n",exception_code);

    longjmp(buf,2);

    printf("Thoat\n");
    return 0;
}
```
khi khời tạo setjmp(buf); giá trị exception_code sẽ bằng 0;
sau khi hệ thống xử lý đến dòng longjmp(buf,2) lệnh này sẽ nhảy đến vị trí khởi tạo ban đầu và thay đổi giá trị exception_code thành 2 chính vì vậy nó sẽ nhảy vào if và in ra dòng nhảy đến 2.

## Xử lý ngoại lệ:
Từ các ứng dụng của 2 hàm trên ta define chúng thành 3 từ khóa chính với cấu trúc TRY CATCH (bắt lỗi và xử lý lỗi) giống với các ngôn ngứ bậc cao như sau.

```c
#define TRY         if((exception_code=setjmp(buf))==0)
#define CATCH(x)    else if(exception_code == (x))
#define THROW(x)    longjmp(buf,(x))
```
Sau đó khi cần xử lý lỗi ta dùng TRY và bắt lỗi ta dùng CATCH. Xác định lỗi, gán giá trị lỗi và nhảy đến xử lý lỗi ta dùng THROW.
```c
#include<stdio.h>
#include<setjmp.h>

jmp_buf buf;
int exception_code;

#define TRY         if((exception_code=setjmp(buf))==0)
#define CATCH(x)    else if(exception_code == (x))
#define THROW(x)    longjmp(buf,(x))

double thuong(int a, int b){
    if(b == 0){
        // longjmp(buf,1);
        THROW(1);   
    }
    return a/(double)b;
}

int checkArray(int array[], int size)
{
    if(size <= 0){
        // longjmp(buf,2);//đặt mã lỗi là 2
        THROW(2);
    }
    return 1;
}


int main(){
    TRY
    {
        int array[0];
        // ketqua = thuong(8,0);
        // printf("Ket qua %f\n",ketqua);
        checkArray(array, 0);
    }
    CATCH(1){
        printf("ERROR! Mau bang 0\n");
    }
    CATCH(2)
    {
        printf("ERROR! Aray bang 0\n");
    }
    return 0;
}
```
</details>

# Lesson 5: Extern - Static - Register - Volatile
<details>
  <summary>Chi tiết</summary>

  ### 1. Extern
  Trong một project thường được chia nhỏ các file để biên dịch riêng lẻ sau đó kết hợp với nhau, việc này nhằm tăng tốc độ biên dịch của các chương trình lớn.
  Vì vậy để sử dụng được một Biến Toàn Cục hay một hàm từ một file khác ta có thể sử dụng "extern" để khai báo lại biến đó trong file cần sử dụng.
  Tóm lại, Extern là: Tham chiếu của một biến toàn cục hoặc 1 hàm nào đó đã được định nghĩa ở phạm vi bên ngoài file đó. (chỉ khai báo chứ không định nghĩa giá trị).
```c
extern <kiểu dữ liệu>  <tên biến>;
extern <kiểu dữ liệu>  <tên hàm>(các parameter truyền vào hàm đó);
```

  ### 2. Static
  Biến static hay còn được gọi là biến tĩnh có 2 loại:
  #### 2.1 Static local variable
  Khi biến được khởi tạo với từ khóa "static" nó sẽ được lưu trữ trong vùng nhớ static,globle và sẽ tồn tại trong suốt vòng đời của chương trình ngay cả khi kết thúc hàm chứa nó. Chính vì vậy khi được khởi tạo lần đầu tiên nó sẽ khởi tạo duy nhất 1 lần, với các lần khởi tạo tiếp theo sẽ bị bỏ qua.
  ```c
//exemple
void count(){
    static int a = 0;
    printf("a = %d\n",a);
    a++;
}
int main(int argv, char const *argc[])
{
    count();
    count();
    count();
    return 0;
}
```
OUTPUT:
```
a = 0
a = 1
a = 2
```
  #### 2.1 Static globle variable and function
  Các biến toàn cục hay hàm sử dụng từ khóa "static" thì khi đó phạm vi sử dụng giới hạn trong File khởi tạo đó, phạm vị bên ngoài file sẽ không thể sử dụng kể cả sử dụng "extern" để truy xuất. Chính bởi vậy nó có tác dụng không cho người dùng khác truy cập từ bên ngoài file vào các hàm phụ hỗ trợ các hàm chính. Đó là ứng dụng để tạo ra các thư viện.
  ```c
static int num;
static void count(){
    int a = 0;
    printf("a = %d\n",a);
    a++;
}
```

  ### 3. Register
  Các biến được khai báo với tử khóa này sẽ được lưu trong thanh ghi register thay vì lưu trong ram, nó sẽ giúp tốc độ tính toán nhanh hơn bình thường

  ### 4. Volatile
  Các biến được khai báo với tử khóa này sẽ thông báo cho chương trình không được phép tối ưu hóa biến đó trong quá trình biên dịch tránh gây lỗi không mong muốn và rất khó phát hiện.
  Một ưu điểm quan trong ứng dụng trong RTOS đó là khi có nhiều luồng chương trình chạy đồng thời, nếu cùng một mảng chạy 2 tác vụ khác nhau khi đang ghi dở vào hàm ở tác vụ 1 mà nhảy sang tác vụ 2 ghi vẫn vào hàm đó thì giá trị ban đầu ghi sẽ bị ghi đè mất và khi quay lại sẽ gây sai dữ liệu. Vì vây ta bắt buộc sử dụng "volatile" để bắt tác vụ đó thực hiện lại từ đầu nếu đang làm dở mà nhảy đi và quay lại.

</details>

# Lesson 6: Bitmask
<details>
  <summary>Chi tiết</summary>
  
  #### Bitmask "mặt nạ bit" là một kỹ thuật sử dụng thao tác với các bit để set, clear, get 1 bit bất kỳ trong 1 thanh ghi bằng cách sử dụng các phép toán logic.
  ### 1. AND bitwise
  - ký hiệu là "&".
  - Được sử dụng để như một phép nhân, với đầu ra là 1 nếu cả 2 bit đầu vào là 1
  
```c
int result = num1 & num2;
```
Example:
```
0 & 0 = 0
0 & 1 = 0
1 & 0 = 0
1 & 1 = 1

  0b11110011
~ 0b11001011
= 0b11000011
```
  ### 2. OR bitwise
  - ký hiệu là "|".
  - Được sử dụng để như một phép cộng, với đầu ra là 0 nếu cả 2 bit đầu vào là 0
  
```c
int result = num1 | num2;
```
Example:
```
0 | 0 = 0
0 | 1 = 1
1 | 0 = 1
1 | 1 = 1

  0b11110011
| 0b11001011
= 0b11111011
```
  ### 3. XOR bitwise
  - ký hiệu là "^".
  - Với đầu ra là 1 nếu 2 bit đầu vào là khác nhau ( 0 với 1)
  
```c
int result = num1 ^ num2;
```
Example:
```
0 ^ 0 = 0
0 ^ 1 = 1
1 ^ 0 = 1
1 ^ 1 = 0

  0b11110011
^ 0b11001011
= 0b00111000
```

  ### 4. NOT bitwise
  - ký hiệu là "~".
  - Được sử dụng để đào toàn bộ các bit của biến đó.
  
```c
int result = ~num;
```
Example:
```
~ 0 = 1
~ 1 = 0
~ 0b11110011 = 0b00001100
```

### 5. Shift left bitwise
  - ký hiệu là "<<".
  - Được sử dụng để dịch chuyển bit sang trái n bit tương đương với x (2^n).
  
```c
int result = num << n;
```
Example:
```
1 << 3 = 0b00000001 << 3 = 0b00001000 = 1 x 2^3
5 << 2 = 0b00000101 << 2 = 0b00010100 = 1 x 2^2 + 1 x 2^4 = 5 x 2^2
```
### 6. Shift right bitwise
  - ký hiệu là ">>".
  - Được sử dụng để dịch chuyển bit sang phải n bit tương đương với / (2^n).
  
```c
int result = num >> n;
```
Example:
```
11 >> 3 = 0b00001011 >> 3 = 0b00000001 = 11 / 2^3 = 1
15 >> 2 = 0b00001111 >> 2 = 0b00000011 = 15 / 2^2 = 3
```

</details>

# Lesson 7: 
<details>
  <summary>Chi tiết</summary>

  ### Tiêu đề phụ 1.1
  Nội dung của tiêu đề phụ 1.1.

  ### Tiêu đề phụ 1.2
  Nội dung của tiêu đề phụ 1.2.

  ### Tiêu đề phụ 1.3
  Nội dung của tiêu đề phụ 1.3.

</details>
