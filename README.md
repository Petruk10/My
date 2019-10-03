# Hubspot form integration with custom multi steps form
If you need to make a multi-step form that will send data to Hubspot contacts, first you need to create a form in Hubspot Forms, and just create the necessary (all) fields there, which should be at all steps of the multi-step form. 
  
If you already have a working multi-step form - that's good. But if not, then you or your developer need to do it. You can do it with ajax so that when you go to the next step, the page does not reload. In my example, for each step I had html pages created with the name part_0, part_1, part_2 and so on, as many steps as the form steps were created. 

After I created all the steps, using js I made the transitions between these steps. For example, in the first step, I have a question and 4 possible answers, after clicking on any answer, the transition to the next step takes place.

It is not necessary to make the form send the data somewhere. You can simply make a pop-up window of thanks and clear the form fields after clicking on the "Submit" button, at the last step.

You can also make steps, for example, through the slider, as it will be easier and more convenient for you.

Now we need to begin the integration of the Hubspot form with our form.

First, in the Hubpot settings of the form, in the "Style and Preview" section, switch to "Set as raw HTML form" so that we can independently use css and js to change the Hubspot form already on our website http://joxi.ru/KAxJloKtZJV6Pm. Next, save all the changes and copy the form code to your page. It is advisable to insert the code somewhere after our form. In the future, we will make it invisible, so the appearance and location do not really bother us. 

Next I will describe the implementation using my example.

At each step, I have 2-4 answers, that is, without a button to go to the next step. The transition occurs when you click on any of the answers. Each answer is made with input radio and a custom label. A transition occurs when a value changes in one of these input.
For a group of inputs, I gave them the same attribute name = step1, in order to look at the change in one of them. The value attribute also needs to be given to everyone with input, but for each its own. Namely value will be transferred to the Hubspot fields of the form.

Next, using js, or rather jQuery, we write the on change event for input. In it, create a variable and assign it the value value of our input.

Now look at the Hubspot form. Through the inspector we find id input-a, into which we need to transfer data for the first step. We take this id and for its value we write our variable which we wrote above. At the end, you need to write .change () to make it work.

An example of my code: 

    $('form').on('change', 'input[name=step1]', function (e) {

      var step1 = $(this).val();
      $('#gesuchter_bereich_hrsv-3cedf7a4-fe39-4088-98c7-273b0547d2ab').val(step1).change();
  
    });

If everything is done correctly, then in the necessary field of the Hubspot form you will see the value of the answer option that you clicked on. 

At the last step, I have 4 fields that need to be filled (Name, Surname, Phone, Mail), a check-box of the privacy policy agreement and a submit button. To all fields (except the checkbox), I gave the same class, so that through one class name I would check all the fields.
I made all fields mandatory, and also in js I registered a check for completeness of all these fields when I click on the submit button. This is necessary so that only after that we would get all the data from these fields together and click on the submit button of the Hubspot form to work in parallel.

Example:

    $('form').on('click', '.stepLastButton', function (e) {
    
      if( ($('.input_final').val() !== '')) && ($('input[name="check_box"]').is(':checked')) ) {
      
        var Name = $('input[name=name]').val();
        $('#firstname-3cedf7a4-fe39-4088-98c7-273b0547d2ab').val(Name).change();
        $('.hs-button.primary.large').click();
        
      }
      
    });

After that, everything should work.
