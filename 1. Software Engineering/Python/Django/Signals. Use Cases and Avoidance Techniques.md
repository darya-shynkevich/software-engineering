Do not use signals when:
1. The signal relates to one particular model and can be moved into one of that model’s methods, possibly called by save().
2. The signal can be replaced with a custom model manager method.
3. The signal relates to a particular view and can be moved into that view.

It might be okay to use signals when:
1. Your signal receiver needs to make changes to more than one model.
2. You want to dispatch the same signal from multiple apps and have them handled the same way by a common receiver.
3. You want to invalidate a cache after a model save.
4. You want to create hooks for a third-party installable app’s interaction with the database. In some cases this can be a better approach than extending objects.
5. You have an unusual scenario that needs a callback, and there’s no other way to handle it besides using a signal. For example, you want to trigger something based on the save() or init() of a third-party app’s model. You can’t modify the third-party code and extending it might be impossible, so a signal provides a trigger for a callback.

## Signal Avoidance Techniques

#### 1. Using Custom Model Manager Methods Instead of Signals

![Pasted image 20231203222741](../../../_Attachments/Pasted%20image%2020231203222741.png)

Then use it like this:

![Pasted image 20231203222758](../../../_Attachments/Pasted%20image%2020231203222758.png)

#### 2. Validate Your Model Elsewhere

If you’re using a pre_save signal to trigger input cleanup for a specific model, try writing a custom validator for your field(s) instead.

#### 3. Override Your Model’s Save or Delete Method Instead

If you’re using pre_save and post_save signals to trigger logic that only applies to one particular model, you might not need those signals. You can often simply move the signal logic into your model’s save() method.

#### 4. Use a Helper Function Instead of Signals

How to test the transition out of signals. Simply follow these steps:
1. Write a test for the existing signal call.
2. Write a test for the business logic of the existing signal call as if it were in a separate function.
3. Write a helper function that duplicates the business logic of the signal, matching the assertions of the test written in the second step.
4. Run the tests.
5. Call the helper function from the signal.
6. Run the tests again.
7. Remove the signal and call the helper function from the appropriate location.
8. Run the tests again.
9. Rinse and repeat until done.

