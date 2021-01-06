1. A way to create a contact form and notify the owner through emal.
2. It is achieved through Google account and Google Sheets
3. It works only with Gmail and with free account it will send 100 mails per 24hours.
	After this the form will still be able to store submitted data but no emails will be sent.
4. The script needs 3 files.
    an HTML form
    a script that will work as connection to POST the form to the Google Sheets
    a script for google Sheets
5. We must use a google sheet and add the script.
6. The basic sheet format must have
    first column named: Timestamp
    the other columns should have the names of the form items
    then in the Tools->Scripts paste the script.
    in the script must change RECEIVER'S EMAIL ADDRESS HERE (line 8) with the email address that mails should be delivered
    Line 94 SHEET NAME HERE should change to sheet name (ie Sheet1)
7. The script has 2 parts, the first is the one that sends the email, the second stores the data to the sheet
8. Alternations have been done so the script produces a success response as text if the mail is sent and an error if not.
9.After we add the script in the Sheet in the script page now, we must use Publich->Publich as a web app and follow the steps.
10. We must: execute this app as Me, and:
11. Who can have access to the app: anyone, even anonymous.
12. Select app version etc.
13. Copy the API KEY that you are going to have after deployment comfirmation.
14. When saving new version of the app go to File->version control->give a name to the newest version.

Make it work:
1. Create an html contact form
2. Method of contact form: POST
3. Action: The API KEY copied from the google sheets app.
	(I prefer having the POST and API KEY in the JS and not in the HTML).
4. After submitting the form will send a respond text, either success for succesfully sent message or error if something went wront.

The tricky part:
When submitting the form you have to use a fetch API like:


document.addEventListener('submit', e => {
fetch(formAction, {
    //formAction is the API KEY 
      method: formMethod,
      // forMethod is "POST"
      //I prefer storing them in variables in the JS file than having them exposed inside the html.
      body: new FormData(e.target)
      //e.target is the form elements collected in one object
      //more on FormData: https://developer.mozilla.org/en-US/docs/Web/API/FormData/Using_FormData_Objects
    })

.then(res => res.text())

    .then(text => new DOMParser().parseFromString(text, 'text/html'))
    
    .then(doc => {
      let responseResult = doc.body.innerHTML;
      console.log(responseResult); //just to check the response.
      if(
        responseResult === "success"){
            ...Do Something
        } else { 
            Do something else 
            }
    )
    })

    .catch(err =>{ Do Something })

    // Prevent the default form submit
  e.preventDefault();

});
