---
title: "The documentation provides troubleshooting information"
---

## Reasoning

We should provide instructions on how to fix a common issue.
If possible, a troubleshooting topic should include a description of the symptom and the steps needed to fix the situation.
This decreases the amount of support requests and improves the user experience.

## Example implementation

```markdown showLineNumbers
## Troubleshooting

### Can’t set up the device

#### Symptom: “This device can’t be reached”

When trying to set up the integration, the form shows the message “This device can’t be reached”.

##### Description

This means the settings on the device are incorrect, since the device needs to be enabled for local communication.

##### Resolution

To resolve this issue, try the following steps:

1. Make sure your device is powered up (LEDs are on).
2. Make sure your device is connected to the internet:
   - Make sure the app of the manufacturer can see the device.
3. Make sure the device has the local communication enabled:
   - Check the device’s settings.
   - Check the device’s manual.
...
 
### I can't see my devices
Make sure the devices are visible and controllable via the manufacturer's app.
If they are not, check the device's power and network connection.

### The device goes unavailable after a day
Make sure you turned off the device's power-saving mode.
```

## Exceptions

There are no exceptions to this rule.
