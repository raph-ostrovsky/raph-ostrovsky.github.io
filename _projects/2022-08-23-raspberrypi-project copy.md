---
title: 'RaspberryPi Project'
subtitle: 'Raport about my RaspberryPi journey'
date: 2022-08-23 00:00:00
featured_image: '/images/rasppi/background.jpg'
---


<script src="https://tryhackme.com/badge/467129"></script>

### Day II

* control Pi's GPIOs with Python

![](/images/rasppi/day_2.jpeg)

```python
import RPi.GPIO as pi
import time

pin = 17

pi.setmode(pi.BCM)
pi.setup(pin, pi.OUT)




while True:
    state = int(input("ON: 1, OFF: 0 any other to exit: "))
    if state == 1:
        pi.output(pin, pi.HIGH)
        time.sleep(2)
    elif state == 0:
        pi.output(pin, pi.LOW)
        time.sleep(2)
    else:
        print("Bye...")
        pi.cleanup()
        break
```

* tried to add a button to control to led - failed

![](/images/rasppi/day_3.jpeg)
