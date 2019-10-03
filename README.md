# Hubspot form integration with custom multi steps form
If you need to make a multi-step form that will send data to Hubspot contacts, first you need to create a form in Hubspot Forms, and just create the necessary (all) fields there, which should be at all steps of the multi-step form. 
  
If you already have a working multi-step form - that's good. But if not, then you or your developer need to do it. You can do it with ajax so that when you go to the next step, the page does not reload. In my example, for each step I had html pages created with the name part_0, part_1, part_2 and so on, as many steps as the form steps were created. 

After I created all the steps, using js I made the transitions between these steps. For example, in the first step, I have a question and 4 possible answers, after clicking on any answer, the transition to the next step takes place.

It is not necessary to make the form send the data somewhere. You can simply make a pop-up window of thanks and clear the form fields after clicking on the "Submit" button, at the last step.

You can also make steps, for example, through the slider, as it will be easier and more convenient for you.

Now we need to begin the integration of the Hubspot form with our form.

First, in the Hubpot settings of the form, in the "Style and Preview" section, switch to "Set as raw HTML form" so that we can independently use css and js to change the Hubspot form already on our website http://joxi.ru/KAxJloKtZJV6Pm. Next, save all the changes and copy the form code to your page. It is advisable to insert the code somewhere after our form. In the future, we will make it invisible, so the appearance and location do not really bother us. 

Далее буду описывать реализацию на своем примере.

У меня на каждом шаге есть 2-4 варианта ответа, то-есть без кнопки для перехода на следующий шаг. Переход происходит при клике на любой из ответов. Каждый ответ у меня сделан с помощью input radio и кастомного label. Переход происходит при смене значения в одном из этих input.
Для группы input-ов я дал им одинаковый атрибут name=step1, что-бы смотреть на изминение на один из них. Атрибут value также нужно дать всем input, но уже для каждого свое. Именно value будет передаваться в поля Hubspot формы.

Далее с помощью js, точнее jQuery, пишем событие on change для input. В нем создем переменную и присваиваем ей значение value нашего input. 

Смотрим теперь на форму Hubspot. Через инспектор находим id input-a, в который нужно передать данные для первого шага. Берем этот id и для его value пишем нашу переменную которую мы написаля выше. В конце нужно написать .change(), что-бы все сработало.

Пример моего кода: 

    $('form').on('change', 'input[name=step1]', function (e) {

      var step1 = $(this).val();
      $('#gesuchter_bereich_hrsv-3cedf7a4-fe39-4088-98c7-273b0547d2ab').val(step1).change();
  
    });

Если все сделано верно, то в нужном поле формы Hubspot Вы увидите значение того варианта ответа, на которые ви кликнули. 

На последнем шаге у меня 4 поля которые нужно заполнить (Имя, фамилия, Телефон, Почта), чек-бокс соглашения политики конфиденциальности и кнопка отправки. Всем полям (кроме чек-бокса), я дал одинаковый класс, что-бы через одно имя класса провериветь все поля.
Все поля я сделал обязательными, и также в js я прописал проверку на заполненность всех этих полей, при клике на кнопку отправки. Нужно для того, что-бы только после этого у нас передались вместе все данные с этих полей и сработал паралельно клик на кнопку отправки формы Hubspot.

Пример:

    $('form').on('click', '.stepLastButton', function (e) {
    
      if( ($('.input_final').val() !== '')) && ($('input[name="check_box"]').is(':checked')) ) {
      
        var Name = $('input[name=name]').val();
        $('#firstname-3cedf7a4-fe39-4088-98c7-273b0547d2ab').val(Name).change();
        $('.hs-button.primary.large').click();
        
      }
      
    });

После этого, все должно работать.
