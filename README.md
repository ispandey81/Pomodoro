# Pomodoro
Pomodoro timer in vue with typescript. Timer is implemented using setInterval method (https://developer.mozilla.org/en-US/docs/Web/API/setInterval).
#### Timer does not work properly if you change tabs or the browser is running in the background. Here are few articles explaining the behaviour:
#### https://hackwild.com/article/web-worker-timers/
#### https://abhi9bakshi.medium.com/why-javascript-timer-is-unreliable-and-how-can-you-fix-it-9ff5e6d34ee0
#### https://maxschmitt.me/posts/setinterval-settimeout-slows-down-on-tab-change/

Due to the unreliable behaviour of the timer I have implemented the timer using a web worker which works accurately - https://github.com/ispandey81/Pomodoro-JS.  
