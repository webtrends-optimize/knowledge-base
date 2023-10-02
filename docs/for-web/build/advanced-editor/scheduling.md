# Scheduling in the Advanced Editor

Scheduling is your ability to choose when the test should be exposed to users. 

If you’d like your test to start at 9am tomorrow, end 2 weeks later, run only for this weekend – these are the sort of problems we have solved with Scheduling in the Advanced Editor.

Outside of these selected times:

- We will not show the transformations to users.
- We will not count users as having entered your test.

## Prerequisites

You will need to be on Tag Version 5.6+. Whilst we have been using this for a while, not all customers will be on one of these versions. 

We have published this to https://wt-lib-v5-6.webtrends-optimize.workers.dev.

In order to use this lib file, you’ll need to add the following code to your tag:
`WT.optimizeModule.prototype.wtConfigObj.libUrl = "https://wt-lib-v5-6.webtrends-optimize.workers.dev";`

If you’re not already using a workers.dev lib file, please make sure it’s compatible with your Content Security Policies before deploying the change.
If you’re using a custom Lib file, we can apply these changes into it for you.

We are happy to help with this process if/as needed.

## Which states does Scheduling apply to?

By default, scheduling only applies to tests which are Live or Paused.

If tests are in staging, scheduling will be ignored as you are likely still building or QAing the test, and we don't want this to get in the way. 

If tests are Published / Complete, scheduling will be ignored as you are likely to have found your winner and chosen to run the winner to all users. 

## Scheduling options

We have two modes – simple (pick a start/end date) or calendar (pick your time slots).

### Simple mode

You have two choices. You can use them in isolation, or together.

![AE Scheduling 1](/assets/ae-scheduling-1.png){style="display:block;"}

#### Start time

You can choose the default value, which is to have no rule, or you can specify your own start date and time.

![AE Scheduling 2](/assets/ae-scheduling-2.png){style="display:block; max-width: 200px;"}

Example scenario: “I want to launch this test at mignight on Monday 2 January 2023”.

Example scenario: "Start the test whenever I launch it".

#### End time

You can choose the default value, which is to have no rule, or you can specify your own end date and time.

![AE Scheduling 3](/assets/ae-scheduling-3.png){style="display:block; max-width: 200px;"}

Example scenario: “I want the test to end at 5pm on Sunday”.

### Calendar Mode

Here, you will be presented with a calendar. You can select individual time slots, whole days, or drag across the calendar for a range or times/range of days.

![AE Scheduling 4](/assets/ae-scheduling-4.png){style="display:block;"}

Example scenario: Run my test every day next week between 6pm and 10pm

## Don't forget

Scheduling only applies once your test is Live, so don't forget to launch it. We won't change the state for you.