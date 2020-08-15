# :e-mail: Automate Reminder Emails for Research Participants

**AutoRemind** is a Python email automation system for sending customizable reminders to participants at different timepoints throughout a study.

## :star: Most Ideal For
This code was written for a very specific study protocol after a personally exasperating experience of manually emailing participants :smile:
This system will be useful if your study is **longitudinal** and comprises of
- Screening participants for *eligibility*
- Sending *different reminder emails* for *several sessions* across a period of time

## :bulb: Features
1) Sends emails to inform participants of their **eligibility**
2) Sends reminder emails to scheduled participants **one day before each session**
3) Sends reminder emails to scheduled participants on the **day of the session**
   - The message template in `autoremind.py` has this as an email reminder to complete a health and travel declaration form on the morning of the experiment.

## :tada: Try it Yourself 
The example csv files in this repository are meant to facilitate trial-and-error with AutoRemind. Simply change the `email` entries in the csv files to your own email address to receive templates of the messages as laid out in `autoremind.py`. You would also need to have a `secret.py` file containing your gmail credentials. If an authentication error occurs, it is most likely because you have to change the [settings](https://myaccount.google.com/u/1/lesssecureapps?pageId=none) in your gmail account to allow access to less secure apps.

### In summary, you should have
- Two separate files containing information of participants who are eligibile vs. ineligible (`example_eligible.csv` and `example_ineligible.csv` respectively)
  - Columns required for email customization:
    - *Name*
    - *Email*
 
- One file containing information of scheduled participants:
  - Columns required for email customization:
    - *Participant Name*
    - *Email*
    - *Phone*
    - Session 1 Details: *Date_Session1*, *Timeslot_Session1*, *Location_Session1*
    - Session 2 Details: *Date_Session2*, *Timeslot_Session2*, *Location_Session2*

If you have more than two sessions in your experiment, you can customize your own code to include them.

### Code Structure

- **Extract participants**:
  - `get_new()`: extracts email addresses of unread emails
  - `get_participants()`: imports csv files for eligible, non-eligible, and scheduled participants
- **Filter participants**:
  - `target_eligibility()` identifies participants to be informed for being eligibility or non-eligible, with the ability to exclude participants who were already informed before.
  - `target_participants()` identifies participants that need to be sent reminders today or tomorrow.
- **Execute Sending**
  - `send_research_info()`: send emails to new email addresses regarding research participation information
  - `send_inform_eligible()`: send emails to participants based on whether they were eligible or not 
  - `send_session_reminder()`: send reminder email one day before respective sessions (for now, the code accommodates only Session 1 and Session 2)
  - `send_declaration_form()`: send health and travel (for COVID-19) declaration forms on the day of the session
 
 ### Execute It
 
These functions are then wrapped in `autoremind()`, which you can choose to control the sending of a certain type of emails.
Running `autoremind.py` will mass send the desired mails to your participants together with ***printed feedback*** on **how many subjects** and **exactly who** were contacted, like so:

- For example, if you only want to send one-day-prior and on-the-day reminders, and eligibility information, set `send_reminders`, `send_eligible` and `send_forms` to `True`. Once you're happy with the customization, you can execute the sending.
```
autoremind(participants_list, silent=False, send_research=False, send_eligible=True, send_reminders=True, send_forms=True)

### Printed feedback ###
2 eligible participants contacted.
3 ineligible participants contacted.
Sending successful eligibility outcome to: ['Subject11', 'Subject12']
Sending unsuccessful eligibility outcome to: ['Subject19', 'Subject20', 'Subject21']
one day before: No session 1 participants to be contacted.
one day before: 1 participants to be contacted for session 2.
Sending reminder emails to: ['Subject_3'].
experiment day: No session 1 participants to be contacted.
experiment day: 2 participants to be contacted for session 2.
Sending health and travel declaration forms to: ['Subject_1', 'Subject_2']
```

- On the other hand, if you only want to send recruitment information to new email addresses (whose emails are still unread), set just `send_research=True`.
  - IMPORTANT: this is assuming ALL unread emails are indeed seeking recruitment information. Filter your inbox appropriately first e.g., make sure you read and reply to other types of emails.

```
autoremind(participants_list, silent=False, send_research=True, send_eligible=False, send_reminders=False, send_forms=False)

### Printed feedback ###
Retrieving unread emails
Sending research recruitment information to: ['insertyouremail1@gmail.com', 'insertyouremail2@e.ntu.edu.sg']
```  


## Other Resources
- [autoemailer.py](https://github.com/colinquirk/autoemailer/) which also greatly inspired this repository!


