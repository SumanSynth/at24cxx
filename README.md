# at24cxx EEPROM Library for STM32

Two-wire Serial EEPROMs AT24Cxx
* Tested AT24C256 on STM32f103c8 dev board

Example:
```
#include "main.h"
#include "i2c.h"
#include "iwdg.h"
#include "at24cxx.h"

#define MEM_ADDR    0x00u

bool writeStatus = false;
bool readStatus = false;
bool eraseStatus = false;

uint8_t  wData[] = "Hello World 123";
uint8_t  rData[25];

int main(void)
{
	HAL_Init();
	SystemClock_Config();
	MX_IWDG_Init();
	MX_I2C2_Init();
	
	HAL_IWDG_Refresh(&hiwdg);
	
	if(at24_isConnected()){
		eraseStatus = at24_eraseChip();
		HAL_Delay(10);
		writeStatus = at24_write(MEM_ADDR,wData, 15, 100);
		HAL_Delay(10);
		readStatus = at24_read(MEM_ADDR,rData, 15, 100);
		HAL_Delay(10);
	}
	
	while (1) {
		HAL_IWDG_Refresh(&hiwdg);
	}
}
```
