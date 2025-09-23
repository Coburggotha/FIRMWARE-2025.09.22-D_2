# FIRMWARE-2025.09.22_23-D_2_3
FIRMWARE-2025.09.22_23-D_2_3
- GNU : General Public License or GNU's Not UNIX
- 운영 체제의 하나이자 컴퓨터 소프트웨어의 모음집

### Node.js 활용


<br>
<br>

- 설치 확인
<p align="center">
  <img width="1116" height="619" alt="image" src="https://github.com/user-attachments/assets/bcb072ac-df4b-4daa-9db8-ae21fad0defa" />
</p>
<br>

- 설치 명령

<p align="center">
<img width="1083" height="293" alt="image" src="https://github.com/user-attachments/assets/d092b736-26f3-4636-94b7-57b78fc4d50b" />
</p>
<br>
- 코드 제너레이터 LED 생성
<p align="center">
<img width="216" height="45" alt="image" src="https://github.com/user-attachments/assets/50cc6a06-005c-476e-a74a-67ac535ae9ef" />
</p>
<br>
- 막 생성된다.
<p align="center">
<img width="387" height="514" alt="image" src="https://github.com/user-attachments/assets/32c2920d-48aa-46d2-b0ef-d15186bb7912" />
</p>
<br>

- 기본의존성? 설치?
<p align="center">
<img width="1090" height="423" alt="image" src="https://github.com/user-attachments/assets/16f953cf-f85c-443a-9c7d-4b7b8984bf91" />
</p>

- 폴더 생성 확인
<p align="center">
<img width="1533" height="1004" alt="image" src="https://github.com/user-attachments/assets/11fa03ce-00d7-48a9-ae87-b3d9b89b1224" />
</p>

- 메모장확인
<p align="center">
<img width="540" height="547" alt="image" src="https://github.com/user-attachments/assets/313c6b8c-1fe3-4a23-baa7-449e5d47789b" />
</p>

- dependencies 자동 생성해준것이다.
- <p align="center">
<img width="237" height="331" alt="image" src="https://github.com/user-attachments/assets/5b6ad8bb-5175-4848-b852-93503ccd197f" />
</p>
- 
<p align="center">
<img width="262" height="74" alt="image" src="https://github.com/user-attachments/assets/f3cafda5-f46a-4f52-b98f-d023aac5a060" />
</p>

- 웹에서 확
<p align="center">
<img width="405" height="199" alt="image" src="https://github.com/user-attachments/assets/98a60678-47d7-450a-a796-c0d17ff9daac" />
</p>

- Notepad 앱으로 확인
<p align="center">
 <img width="1003" height="321" alt="image" src="https://github.com/user-attachments/assets/0ea6aaeb-9648-48a5-a79d-c053cf911d4a" />
 </p>
 
<p align="center">
 <img width="993" height="534" alt="image" src="https://github.com/user-attachments/assets/e0d5d3da-43d4-46d2-8154-a1293829d2b1" />
  </p>

  - 비트마스크
  - x & 1 = x 
  - x & 0 = 0 (비트 클리어)

 - x | 0 =x
 - x | 1 =1 (SET)

### 3

```c
* Includes ------------------------------------------------------------------*/
#include "main.h"

/* Private includes ----------------------------------------------------------*/
/* USER CODE BEGIN Includes */
#include <stdio.h>
/* USER CODE END Includes */

/* Private typedef -----------------------------------------------------------*/
/* USER CODE BEGIN PTD */

/* USER CODE END PTD */

/* Private define ------------------------------------------------------------*/
/* USER CODE BEGIN PD */

/* USER CODE END PD */

/* Private macro -------------------------------------------------------------*/
/* USER CODE BEGIN PM */

/* USER CODE END PM */

/* Private variables ---------------------------------------------------------*/
TIM_HandleTypeDef htim2;    // TIM2 타이머 주변장치를 제어하기 위한 HAL 핸들 -> htim2 로 타이머 주변장치를 설정한다

UART_HandleTypeDef huart2;  // USART2(UART2) 주변장치를 제어하기 위한 HAL 핸들 -> Uart2 로 타이머 주변장치를 설정한다

/* USER CODE BEGIN PV */

/* USER CODE END PV */

/* Private function prototypes -----------------------------------------------*/
void SystemClock_Config(void);
static void MX_GPIO_Init(void);
static void MX_USART2_UART_Init(void);
static void MX_TIM2_Init(void);
/* USER CODE BEGIN PFP */

/* USER CODE END PFP */

/* Private user code ---------------------------------------------------------*/
/* USER CODE BEGIN 0 */
#ifdef __GNUC__
/* With GCC, small printf (option LD Linker->Libraries->Small printf
   set to 'Yes') calls __io_putchar() */
#define PUTCHAR_PROTOTYPE int __io_putchar(int ch)
#else
#define PUTCHAR_PROTOTYPE int fputc(int ch, FILE *f)
#endif /* __GNUC__ */

/**
  * @brief  Retargets the C library printf function to the USART.
  * @param  None
  * @retval None
  */
PUTCHAR_PROTOTYPE
{
  /* Place your implementation of fputc here */
  /* e.g. write a character to the USART1 and Loop until the end of transmission */
  if (ch == '\n')
    HAL_UART_Transmit (&huart2, (uint8_t*) "\r", 1, 0xFFFF);
  HAL_UART_Transmit (&huart2, (uint8_t*) &ch, 1, 0xFFFF);

  return ch;
}

void HAL_TIM_PeriodElapsedCallback(TIM_HandleTypeDef *htim){
	if(htim->Instance == TIM2){
		printf("Timer interrupt start\n\r");
	}
// 사용하는 htim, 즉 타이머가 htim2이면(==TIM2) "Timer interrupt start" 출력

void HAL_GPIO_EXTI_Callback(uint16_t GPIO_Pin){

	static uint32_t last_time = 0;  // 부호없는 32비트 정적변수 last_time 선언 하고 0으로 초기화
                                  // 정적변수는 블록을 남아도 남아있는 전역변수임
	uint32_t current_time = HAL_GetTick(); // 부호없는 32비트 변수 current_time을 선언하고 HAL_GetTick() 으로 초기화
                                         //  HAL_GetTick() 는 1ms 랜다..
	if((current_time - last_time) > 100){  // current time - last_time이 100보다 크면 if 문 실행
		if(GPIO_Pin == B1_Pin){              // 인터럽트 핀이 B1_Pin이면 실행
			current_time = last_time;          // current_time에 last_time을 대입;
			if(HAL_GPIO_ReadPin(B1_GPIO_Port, B1_Pin) != GPIO_PIN_SET){ // 읽은 값이 1이 아니면(0이면) 실행
				printf("Timer Stop \n\r");
				HAL_TIM_Base_Stop_IT(&htim2);   //  TIM2 베이스 타이머 인터럽트 모드 정지
			}
			else{                         
				printf("Timer restart\n\r");
				HAL_TIM_Base_Start_IT(&htim2); // TIM2 베이스 타이머 인터럽트 모드 실
			}

		}

	}

}



/* USER CODE END 0 */

/**
  * @brief  The application entry point.
  * @retval int
  */
int main(void)
{

  /* USER CODE BEGIN 1 */

  /* USER CODE END 1 */

  /* MCU Configuration--------------------------------------------------------*/

  /* Reset of all peripherals, Initializes the Flash interface and the Systick. */
  HAL_Init();

  /* USER CODE BEGIN Init */

  /* USER CODE END Init */

  /* Configure the system clock */
  SystemClock_Config();

  /* USER CODE BEGIN SysInit */

  /* USER CODE END SysInit */

  /* Initialize all configured peripherals */
  MX_GPIO_Init();
  MX_USART2_UART_Init();
  MX_TIM2_Init();
  /* USER CODE BEGIN 2 */
  HAL_NVIC_SetPriority(EXTI15_10_IRQn, 0,0);
  HAL_NVIC_SetPriority(TIM2_IRQn, 15, 0);



  printf("--------Interrupt Test--------\n\r");

  HAL_TIM_Base_Start_IT(&htim2);
  /* USER CODE END 2 */

  /* Infinite loop */
  /* USER CODE BEGIN WHILE */
  while (1)
  {

    /* USER CODE END WHILE */

    /* USER CODE BEGIN 3 */
  }
  /* USER CODE END 3 */
}
```

### 3. ADC_VR 가변저항 값을 ADC로 읽는 프로젝트
- 프로젝트 생성
- PA0 핀을 ADC 입력 핀으로 사용하기 위해 설정한다. MCU가 3.3V 동작이기 때문에 가변저항 VCC는 3.3V에 연결한다.
<p align="center">
 <img width="655" height="640" alt="image" src="https://github.com/user-attachments/assets/48efe612-f660-4afc-9ca2-15250156eaa0" />
</p>
<br>
<br>

- 기본 설정은 기본값을 유지 한다.
<p align="center">
<img width="602" height="373" alt="image" src="https://github.com/user-attachments/assets/c5e093f9-0e05-4d2d-b991-a02e579d90bf" />
</p>
<br>
<br>

- Rank 아래 Sampling Cycle을 13.5로 바꾸어준다.
- Sampling Cycle은 입력전압을 확인하는 시간(샘플링시간)이다. 짧을 수록 빠르지만 정확성이 낮고 길수록 느리지만 정확성이 높다.
<p align="center">
<img width="600" height="418" alt="image" src="https://github.com/user-attachments/assets/dbdbbdcb-4808-4749-95d6-30bb5c22a1c5" />
</p>
<br>
<br>

- 샘플링 시간을 13.5로 바꿔서 클럭 이슈가 생기는데 이를 자동으로 해결할 것이냐는 문구가 뜬다. YES
<p align="center">
<img width="521" height="271" alt="image" src="https://github.com/user-attachments/assets/3523ed98-4aef-4f76-9b46-b49e6456b1d2" />
</p>
<br>
<br>

- 그래도 문제가 남아있다면 Resolve Clock Issues를 누른다.
 <p align="center">
<img width="1122" height="858" alt="image" src="https://github.com/user-attachments/assets/9122090f-bcf5-43f6-bb90-e11c03165ac5" />
</p>
<br>
<br>

-해결완료
 <p align="center">
<img width="1141" height="859" alt="image" src="https://github.com/user-attachments/assets/9bc9c83a-3fb4-4e62-b693-6e20db2628b0" />
</p>
<br>
<br>

- 우선 결과를 시리얼로 통신하여 문자로 확인하기 위해 printf를 위한 헤더파일 추가와 코드를 추가한다.
```c
/* USER CODE BEGIN Includes */
#include <stdio.h>
/* USER CODE END Includes */

/* USER CODE BEGIN 0 */
#ifdef __GNUC__
#define PUTCHAR_PROTOTYPE int __io_putchar(int ch)
#else
#define PUTCHAR_PROT0TYPE int fputc(int ch, FILE *f)
#endif
PUTCHAR_PROTOTYPE {
	if (ch == '\n')
		HAL_UART_Transmit(&huart2, (uint8_t*) "\r", 1, 0xFFFF);
	HAL_UART_Transmit(&huart2, (uint8_t*) &ch, 1, 0xFFFF);

	return ch;
}
/* USER CODE END 0 */

```
<br>
<br>

- 데이터를 받기 위한 변수를 선언한다.
  ```c
  /* USER CODE BEGIN PV */
uint32_t last_adc_value = 0;  // 32비트 부호 없는 정수 last_adc_value를 선언하고 0으로 초기화

float voltage = 0.0;         // 부동소수 voltage를 선언하고 0.0으로 초기화

/* USER CODE END PV */
```
<br>
<br>

- ADC 켈리브레이션 ADC를 초기화 시키는 코드 작성
```c
/* USER CODE BEGIN 2 */
	if (HAL_ADCEx_Calibration_Start(&hadc1) != HAL_OK) {

		Error_Handler();
	}
	/* USER CODE END 2 */

```
<br>
<br>

- 4515
  ```c
  /* USER CODE BEGIN WHILE */
	while (1) {

		uint32_t current_adc_value;

		HAL_ADC_Start(&hadc1);

		if (HAL_ADC_PollForConversion(&hadc1, 100) == HAL_OK) {

			current_adc_value = HAL_ADC_GetValue(&hadc1);
			voltage = 3.3*current_adc_value/4095;

			if (abs(current_adc_value - last_adc_value) > 10) {
				printf("Potentiometer ADC Value: %lu\r\n", current_adc_value);
				printf("Potentiometer voltage: %.2f\r\n", voltage);
				last_adc_value = current_adc_value;
			}

		}
		/* USER CODE END WHILE */
		HAL_Delay(500);
		/* USER CODE BEGIN 3 */
	}
  ```
<br>
<br>



  
