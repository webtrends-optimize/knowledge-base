# States

There numerous states in Optimize, each with their own set of properties.

## Pending 

In this state, users will not be allowed to see the experience (by any means). It is removed from our servers (OTS) and so is not even possible to be evaluated and delivered.

## Staging

In this state, users will only be given the experience if they are in Staging Mode. To enter staging mode, you can use the Force Experiment Widget, or add `?_wt.mode=staging` to the query string.

### What users will see in staging mode?

You can hop into any variation using either Preview Links or the Force Experiment Widget. 

They will be delivered in an identical way to Live experiences - we do not transform your code, or deliver it through a different method.

### Will metrics track in staging mode? 

If they are in Preview, you won't get any metrics tracking.

If not in Preview, metrics will capture and be available for QA / validation before going live.

## Live 

In this state, users will only be given the experience if they are in Normal Mode. Users to your website will be in this state by default - you only leave it to enter Staging Mode, which you typically do intentionally. 

To enter normal mode if you're in staging, you can use the Force Experiment Widget, or add `?_wt.mode=normal` to the query string.

### What users will see in normal mode?

Users will be assigned your experience as you'd expect - being evaluated for Location, Segment and then being given your code.

#### Bucketing users / stickyness

There are differences to appreciate, between tests (Baseline, AB, ABn, MVT) and Targets, when it comes to bucketing, stickyness and evaluation. 

**Targets evaluate on every page load**, and so users can fall out of a target on their next page load if they're no longer elibile. This may be the case based on no longer matching Segments, or if the Target is Paused.

This also applies to which variation they're assigned - Targets are not sticky, and so users can be assigned a different variation on each page load, depending on which rule they match.

**Tests are sticky**. Differently to targets, once you are assigned a variation for a test, note that: 

- You will stay in the test, and not fall out of it even if you no longer match the Segment rule. 
- You will always be assigned that variation, and this assignment will not change.

### Will metrics track in live mode? 

Yes. As expected. Data typically rolls up into the UI within 5-10 minutes, and all queries are based on this "live" dataset. It's not cached, grouped or released in stages as you may see in other platforms.

## Paused 

Anything in the paused state will not be shown to users - both for tests and targets. Metrics will be captured for tests users fell into in the past, but bucketing will stop at that point and so metrics will tail-off over time.

## Published / Complete 

This state is only available for tests - AB, ABn or MVT. 

This is typically used in one situation - where your test has reached it's conclusion, and you want to "pin" an experiment to show it to all users who pass your Segmentation check.

If you hop straight to this state when building your test, your use-case is likely better suited to running Targets with a single variation.

## Archived

This is a final state, and tests cannot transition into anything else from here. If you need to revive an archived test, you'll need to Clone it and then use that Clone instead.

As with Pending, anything in the Archived state will not exist in our servers and as such users will not be evaluated for these experiences.

## Keywords
test state 
target state 
changing state 
pending staging live pause paused published archived