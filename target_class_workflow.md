---

layout: minimal
title: Adobe Target - Classic Workflow

---
# Adobe Target: Classic Workflow
Author: Jason Thompson

__Problem__: As organizations mature in their use of optimization platforms, such as Adobe Target, keeping tests flowing smoothly through development, QA, and production releases can often become a real challenge.

__Actions__: We will walk through sample workflow, using Adobe Target Classic, to illustrate how organizations can run multiple, concurrent, campaigns and still maintain flexibility in managing their testing queue.

__Explanation (resolution)__: Let's begin by creating a new Campaign that will be used for development and QA purposes.

![Adobe Target - Create New Campaign](/images/target_new_campaign.png)

Our campaign naming syntax contains 'the date the campaign was created' - 'campaign description' - '{optional environment}  We also use of Tags to bucket campaigns as QA or Production so they are easy to locate as well.

We typically use 'Landing Page Campaign' for our QA campaigns as this Campaign Type allows us to qualify for multiple experiences, contrasted to a typical A/B...N that locks you into the first experience you see -- forcing you to delete cookies when testing so you can qualify for a different experience.  

If should be noted that we keep these campaigns live, even after launching the production version of the campaign, in case there is a need to investigate any issues with the live campaign or if a key stakeholder wants to see what version b looks like. Having the QA campaign live makes it super easy to do that.

Next will with add targeting at the Location (mBox) level.

![Adobe Target - Target Location](/images/target_target_location.png)

Here is where the use of query string parameters come into play. At the Location (mBox) level, we add a Target that looks for a name:value pair. In our case, we always use 'campaign' as the name and the value is typically a term that identifies the specific campaign. In this example, since we are running a test on the site home page, we made the name equal 'shp'.

Now we will add targeting at the Experience level.

![Adobe Target - Target Location](/images/target_target_experience.png)

This is where we make use of query string parameters to determine what Experience we want to be included in. Again we will make use of a name:value pair to qualify for a specific experience. The name we use is 'v' (for version) and for the control we use 'control' for the value. We will use the same format to target the test experience(s), in this example, we have only one test experience that we will call 'cta' as shown in the screenshot above.

Once you have this completed and have selected a Conversion Location, you can save and approve you campaign.

Now that we have an approved campaign, we can build links that we can distribute to the team to view, QA, and validate the campaign.

**Example**:

Control: http://www.site.com/?campaign=shp&v=control
Homepage CTA: http://www.site.com/?campaign=shp&v=cta

Once testing has been completed and the business has signed off on the test, we can promote our test to production a.k.a "make it live." To do this, we will make a copy of the QA campaign (but leave the campaign active) to create a production ready version.

![Adobe Target - Target Location](/images/target_production_campaign.png)

Make the following changes to the campaign setup:

1. Update the date to reflect the production launch date and remove the QA identify if one was used.

2. Change the Campaign Type from Landing Page to A/B...N

3. Remove the QA Targeting rules at the Experience level e.g. v=control

4. Update the Targeting rule at the Location (mBox) level to be 'does not contain campaign=' What this does is help keep Production and QA campaigns separated, which can often be a challenge when you are running several campaigns at once. This step isn't required but I have found that is has helped keep things cleaner.

![Adobe Target - Target Location](/images/target_production_exclude.png)


#### References

[Adobe Target Product Documentation](https://marketing.adobe.com/resources/help/en_US/target/)
