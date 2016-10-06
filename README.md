# Salesforce Durable PushTopic Streaming Demo
This repository contains all the code you need to set up a Durable PushTopic Streaming client inside of a Visualforce page in your Salesforce org.  Durable PushTopic Streaming adds the capability to replay any of the messages sent to the PushTopic for the last 24 hours.

This is handled through the use of an Replay ID.  Every event sent through the PushTopic Streaming system will have an associated ID.  With Durable PushTopic Streaming, you will have the ability to provide a Replay ID (ostensibly the last ID that your client has received) and the system will send every event that has happened in the last 24 hours after that specific ID.

If you do not provide an ID, or the ID you provide is -1, your client will be subscribed to the tip of the queue (you will receive all new messages).  If you provide an ID of -2, we resend ALL events that happened in the last 24 hours.

## Demo Instructions
1. Download this repo as zip file
    * This contains all of the needed code to run the client inside of Salesforce
2. Deploy the code to an enabled Spring '16 org using the MdAPI through Workbench, Force.com IDE, or Ant Migration Tool
3. Assign the included StreamingDurablePushTopicDemo permission set to your user
4. Create the `/topic/TestAccountStreaming` PushTopic by navigating to `/apex/StreamingDurablePushTopicDemo` - this Visualforce page will create the topic then auto-subscribe to `/topic/TestAccountStreaming` using Durable PushTopic Streaming.
5. Create notifications for the new topic
    * Use the on-screen utility to create, update and then delete an Account record
    * Or, use the UI or API to create, update, delete or undelete an Account record  
6. Observe events being received in bottom panel
7. Update replay subscription settings by changing the Replay From attribute. This will change where in the event stream you are subscribed to
    * -2 will replay from eariest available event
    * -1 (or no ID) will replay only new events
    * Provide a specific Replay ID to replay all events after that specific ID

## Open Issues to Remember
1. This is still a Salesforce Beta Feature
    * It needs to be enabled manually on every org
    * Issues should be communicated directly to the Product Manager, Jay Hurst (jhurst@salesforce.com)
2. You cannot replay using an ID outside of your event stream window (if you provide an ID from 2 weeks ago, it will be invalid)
3. The current event stream window is 24 hours
