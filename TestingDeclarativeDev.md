### [How do I write Apex tests for Salesforce Flows and Process Builders?](https://www.desynit.com/good-systems-blog/salesforce/writing-apex-tests-for-salesforce-flows-and-process-builders/)

Winter ‘19 saw the exposure of the “FlowTestCoverage” object via the Tooling API… which told us all the actually Salesforce are clocking the Test Coverage of declarative Processes and Flows.. Who knew it?!

As of now, it is just the result of Apex tests and how they cover these business processes; but if I poke my neck out of the nice safe harbour, it feels like this might be the first step towards provisioning a declarative testing tool – and maybe even slowly introducing a requirement for finally having tests to assert the behaviour of these unbelievably powerful entities (though as it seems like Salesforce are winding down on required tests for even code, perhaps these will never be mandatory).

So to get ahead of the game, I thought I would look quickly at how one might go ahead getting coverage of an auto-invoked Flow, fired from a Process Builder.

We are still stuck with code to do this for now (October 2018) and so the experience is not too different to testing other event based technologies like Triggers.

So let’s look at something we want to test:

Our scenario is that we have a Process Builder that fires when a specific field is updated on an Opportunity, which populates a “Opportunity Value” on a child “Tracking Record”. We will assume that some other automation creates the default Tracking Record when an Opportunity is first saved.

So our process might look like this:

![Process Kicking Flow](https://www.desynit.com/wp-content/uploads/2018/10/Simon-image-one.png)

Which invokes a flow (with the opportunity Id as an input parameter) that looks like this:

![Flow to Invoke](https://www.desynit.com/wp-content/uploads/2018/10/Simon-image-2.png)

How do we test this then? Well, we need to cause our Process Builders event to happen, which is to “update” an Opportunity appropriately, and then check that the last Tracking Record has been updated. We could all do this through the browser, but how do we make the System check it on our behalf?

Let’s write some code! Fire up the Developer Console, or your favourite IDE, and let’s get testing.

First off we need to make a new Apex Class, annotated as being a test; it is a good convention to name the test after the unit it is testing (in this case the “OpportunityTracking” process and flow). Then, for this test, we are going to need an Opportunity, so we create and insert one of these in a “testSetup” method. This will look something like this:

    @isTest
    public class OpportunityTrackingProcessBuilderTest {
    
    @testSetup
    private static void InsertTestOpp() {

	   // Construct a test Opportunity
        Opportunity opp = new Opportunity(Name = 'test opportunity',
                                          StageName = 'Prospecting',
							    Tracked_Value__c = 1,
                                          CloseDate = Date.today());

	   // Save this Opportunity to the database
        insert opp;
    }

    // Our test methods now go here

    }
