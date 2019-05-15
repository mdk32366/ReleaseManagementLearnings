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
If we were to run this test class now, it would execute, and it will already “cover” some stuff, as it is going to insert an Opportunity, which will fire that Process to create the default tracking record (for example), but what we need to do is write some code to fire our event – which is to change the Tracked_Value__c amount.

So we write a @testMethod, in the same class (as mentioned in the last comment) to load, change and save this Opportunity. That should then cause the Process builder to fire – just like if we did it through the browser:

    @isTest
    private static void UpdateOpportunity_ChangeTracked_ProcessFired() {
        
    // Load the Opportunity:
    Opportunity loadedOpp = [SELECT Tracked_Value__c
                             FROM Opportunity];
        
    // Set the Tracked value to something else:
    loadedOpp.Tracked_Value__c = 5;
        
    // Save it to the database, and fire the process builder
    update loadedOpp;
        
    }
    
So, there we go! In essence, our test class – when executed – now will insert an Opportunity, update it and save it again, firing our Process Builder. If we run this test and the whole of California doesn’t burst into flames, we can probably suppose that our Process Builder and Flow was ok.

However, supposing stuff isn’t very reliable. What we want to do is assert it. So we would now add a couple of checks to the bottom of our test method to make sure the output we were expecting actually happened. In this case, we will want to load the Opportunity’s tracking record and check it’s Opportunity Value is now the new “5”. To do that, we add some more code to our test.  
    
    @isTest
    private static void UpdateOpportunity_ChangeTracked_ProcessFired() {
        
    // Load the Opportunity:
    Opportunity loadedOpp = [SELECT Tracked_Value__c
                             FROM Opportunity];
        
    // Set the Tracked value to something else:
    loadedOpp.Tracked_Value__c = 5;
        
    // Save it to the database, and fire the process builder
    test.startTest();
        update loadedOpp;
    test.stopTest();
        
    // Load the tracking record for this Opportunity
    Tracking_Record__c trackingResult = [SELECT Opportunity_Value__c
                                         FROM Tracking_Record__c
                                         WHERE Id = :loadedOpp.Id];
        
    // Check the tracked value is now 5
    System.assertEquals(5, trackingResult.Opportunity_Value__c);
    }

This test actually is now fundamentally complete. It would be a fairly acceptable piece of work from a new developer and should reliably assert that the behavior of this Process and Flow are working. If someone changed the Flow or Process you may well find this test starts failing, which could indicate to you that this had happened and you might want to question why.

Now then, what other scenarios might we want to test here? It is not a very rigid test, and only tests the “correct” case. You can write many test methods in the same class (reusing a fresh version of the test Opportunity each time).

Which of the following scenarios might you also want to write a test method for to make sure nothing goes wrong in your Org…? Probably not all of them, but they are all food for thought!

The Opportunity is updated, and every field other than Tracked_Value__c is changed
The Opportunity is updated, so the Tracked_Value__c is changed to BLANK
A dataload is performed causing 1500 Opportunity Tracked_Value__c fields to change
The Opportunity is updated, and the Tracked_Value__c is changed from 1 to 1.00000
The Opportunity, with a Tracked_Value__c is deleted, and/or undeleted
The Opportunity has its Tracked_Value__c updated, and saved, and then updated and saved again, and then updated and saved again.
