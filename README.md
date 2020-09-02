<!DOCTYPE html>
<html lang="ru">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <link rel="shortcut icon" href="img/favicon.ico" type="image/x-icon">
    <title>TEST</title>


</head>

<body>

    <main>

        <div class="box">


            <div class="timer">
                <span class="day">00</span>
                <span class="hour">00</span>
                <span class="min">00</span>
                <span class="sec">00</span>
            </div>


        </div>

    </main>

    <script>

        function timerLogic(dayItem, hourItem, minItem, secItem) {

            let dayBox = document.querySelector(dayItem),
                hoursBox = document.querySelector(hourItem),
                minBox = document.querySelector(minItem),
                secBox = document.querySelector(secItem);

            let endTime,
                interval,
                count = 1; // чтобы при перезагрузке страницы запись в local не обновлялась, устанавливаем счетчик со значением 1 и при первом запуске таймера меняем его на 2. В функцию запуска скрипта вставляем проверку, если это значение 1 то делаем запись в local

            // условие чтения из local. 
            if (localStorage.getItem('timer')) { // если в localStorage есть запись с ключом - timer
                endTime = new Date(localStorage.getItem('timer')); // в endTime записываем время окончания работы таймера
                timer();
                interval = setInterval(timer, 1000);
            }

            // функция запуска скрипта
            function onStart() {
                if (count == 1) {
                    endTime = Date.now() + 172800000; // устанавл. время окончания работы таймера (172800000 - это кол-во миллисекунд в 48 часах. это число меняется, в зависимости от того, на сколько нужно выставить таймер)
                    localStorage.setItem('timer', new Date(endTime).toISOString()); // записываем таймер в local 
                    timer();
                    interval = setInterval(timer, 1000);
                }
            }

            // таймер
            function timer() {

                count = 2;

                let t = endTime >= Date.now() ? Math.round((endTime - Date.now()) / 1000) * 1000 : 0;

                let days = Math.floor((t / (1000 * 60 * 60 * 24)));
                dayBox.innerHTML = (days < 10 ? "0" : "") + days;

                let hours = Math.floor((t / (1000 * 60 * 60)) % 24);
                hoursBox.innerHTML = (hours < 10 ? "0" : "") + hours;

                let minutes = Math.floor((t / 1000 / 60) % 60);
                minBox.innerHTML = (minutes < 10 ? "0" : "") + minutes;

                let seconds = Math.floor((t / 1000) % 60);
                secBox.innerHTML = (seconds < 10 ? "0" : "") + seconds;

            }

            // по загрузке страницы запускаем скрипт
            document.addEventListener('DOMContentLoaded', function () {
                onStart();
            });

        }

        timerLogic('.day', '.hour', '.min', '.sec');

    </script>

</body>

</html>
