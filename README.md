# L476-PWM2-ContinuousDutyCycle
## 📌 Project Overview
This project demonstrates how to generate a PWM signal using STM32 (TIM2 Channel 2) and dynamically adjust the duty cycle from 0% to 100% and back.

The duty cycle is gradually increased and decreased in a loop, creating a smooth fading effect (e.g., LED breathing).

---

## ⚙️ Hardware Requirements
- STM32 development board (e.g., STM32L4 series)
- Oscilloscope / Logic Analyzer (optional)
- LED + Resistor (optional)

---

## 🧠 Key Features
- PWM generation using TIM2
- Real-time duty cycle adjustment
- Smooth ramp-up and ramp-down effect
- HAL-based implementation

---

## 🏗️ Configuration Details

### 🕒 Timer Settings (TIM2)
| Parameter     | Value |
|--------------|------|
| Prescaler     | 79   |
| Period (ARR)  | 99   |
| Counter Mode  | Up   |

### 📊 PWM Formula

f_PWM = Timer Clock / ((Prescaler + 1) * (Period + 1))
Duty = CCR / ARR


---

## 🔧 Core Function

```c
void DutyCycle(float Duty){
    uint16_t AutoReload, SetDutyCycle;
    AutoReload = __HAL_TIM_GET_AUTORELOAD(&htim2);
    SetDutyCycle = AutoReload * Duty / 100.0;
    __HAL_TIM_SET_COMPARE(&htim2, TIM_CHANNEL_2, SetDutyCycle);
}

👉 Convert duty percentage (0–100%) into CCR value.

🔁 Main Loop Behavior
HAL_TIM_PWM_Start(&htim2, TIM_CHANNEL_2);

while (1)
{
    for(int i = 0; i <= 100; i++){
        DutyCycle(i);
        HAL_Delay(100);
    }
    for(int i = 100; i >= 0; i--){
        DutyCycle(i);
        HAL_Delay(100);
    }
}
💡 Behavior
0% → 100% → 0%
Continuous loop
Produces fading / breathing effect
📈 Output Behavior
Signal: PWM square wave
Frequency: constant
Duty cycle: changing
Result:
Oscilloscope → pulse width varies
LED → brightness fades

