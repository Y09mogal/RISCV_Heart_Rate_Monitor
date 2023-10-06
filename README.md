# RISCV_Heart_Rate_Monitor

## Heart Rate Monitor
In the post covid times, everyone wishes and needs to be aware of their vitals every often. Hence, we propose to you the Heart Rate Monitor built using the MAX30201 sensor and a custom RISCV32 core specifically designed for it. Here, the sensor sends the vitals data to ADC to make it digitally available for the custom RISCV32 core deployed on the board via I2C communication protocol. It then processes the signals, calculates the heart rate in BPM, and displays it on the seven-segment displays. This project can be further expanded to calculate SpO2 vitals using the same sensor and measures temperature. Utilizing other GPIOs on the board, one can extend this project to make a personal health monitoring device which not only calculates Heart Rate and SpO2 vitals but also Sleep monitoring, emotion state detection for mental well-being, Body fat analyser in the form of BMI to keep the track of fitness of the individual, etc. 



## Acknowledgments
* Kunal Ghosh, CEO, and Co-founder, VSD Corp. Pvt. Ltd.
* Mayank Kabra, IIITB
* Aamod BK, IIITB
* Saket Gurjar, IIITB

## Contact Information
* Yash Dharemsh Mogal, IMtech 2020, International Institute of Information Technology, Bangalore Yash.Mogal@iiitb.ac.in
* Kunal Ghosh, CEO, and Co-founder, VSD Corp. Pvt. Ltd. kunalghosh@gmail.com


## References
- https://github.com/SakethGajawada/RISCV_GNU
- https://github.com/sparkfun/SparkFun_MAX3010x_Sensor_Library
- https://how2electronics.com/blood-oxygen-heart-rate-monitor-max30100-arduino/
- https://chat.openai.com
