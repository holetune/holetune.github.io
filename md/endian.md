# endian(ness)
## 同義語
### byte order
## 小話
- ジョナサン・スウィフトの風刺小説『ガリヴァー旅行記』の中のエピソードに由来
- 第1部「小人国」では
- ゆで卵を丸い方（大きい方）の端 (big end) から割る人々（Big Endians）と
- 尖った方（小さい方）の端 (little end) から割る人々 (Little Endians) との対立
## Pro・Con
### big endian
- 人間が普段慣れている記法(memory dump)と同じ順序
- アドレスの上位で送り先のおおまかな識別ができる場合
- キャリアグレードルータなどではデータを受信しながら処理を始めることができる
- TCP/IPプロトコルスタックで採用。ネットワークバイトオーダ
### little endian
- 数値の表現法と対応しているという整合性
- データの送受信で多倍長加算などで最下位から順番に計算したほうが繰上りの計算がしやすい
## ワード単位
### 4byte data
- 12 34 56 78 : big endian
- 78 56 34 12 : little endian
- 34 12 78 56 : middle endian
- 56 78 12 34 : middle endian
### 8byte data
- 12 34 56 78 9A BC DE F0 : big endian
- F0 DE BC 9A 78 56 34 12 : little endian
### 任意：例.bit
- 以下のほうが安全な(誤解のない)表現
- MSB 1st
- LSB 1st

## 64bit 実例
```c:64bit
#include <inttypes.h>
#include <stdint.h>
#include <stdio.h>

int main(int argc, char **argv)
{
    union
    {
        uint64_t b8;    /* 8 bytes */
        uint32_t b4[2]; /* 4 bytes x 2 */
        uint16_t b2[4]; /* 2 bytes x 4 */
        uint8_t  b1[8]; /* 1 byte  x 8 */
    } bytes;

    bytes.b8 = UINT64_C(0x123456789ABCDEF0);
    printf("bytes.b8: %16" PRIX64 "\n", bytes.b8);
    printf("bytes.b4: %08" PRIX32 ", %08" PRIX32 " \n", bytes.b4[0], bytes.b4[1]);
    printf("bytes.b2: %04" PRIX16 ", %04" PRIX16 ", %04" PRIX16 ", %04" PRIX16 " \n", bytes.b2[0], bytes.b2[1], bytes.b2[2], bytes.b2[3]);
    printf("bytes.b1: %02" PRIX8 ", %02" PRIX8 ", %02" PRIX8 ", %02" PRIX8 ", %02" PRIX8 ", %02" PRIX8 ", %02" PRIX8 ", %02" PRIX8 " \n",
        bytes.b1[0], bytes.b1[1], bytes.b1[2], bytes.b1[3], bytes.b1[4], bytes.b1[5], bytes.b1[6], bytes.b1[7]);
    return 0;
}
```
```output
 ./main
bytes.b8: 123456789ABCDEF0
bytes.b4: 9ABCDEF0, 12345678 
bytes.b2: DEF0, 9ABC, 5678, 1234 
bytes.b1: F0, DE, BC, 9A, 78, 56, 34, 12 
```