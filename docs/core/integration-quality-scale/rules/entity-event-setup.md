---
title: "Entity events are subscribed in the correct lifecycle methods"
---

## Reasoning

Entities may need to subscribe to events, eg. from the integration library, and update state when a new event comes in.
In order to do this correctly, the entities should subscribe and register the update callback in the entity method `async_added_to_hass`.
This entity method is called after the entity has been registered by the entity platform helper and the entity will now have all its interfaces available to call, such as `self.hass` and `self.async_write_ha_state`.
Registering an update callback before this stage will cause errors if the callback eg. tries to access `self.hass` or write a state update.
To avoid memory leaks, the entities should unsubscribe from the events, ie. unregister the update callback, in the entity method `async_will_remove_from_hass`.

## Example implementation

In the example below, the `self.client.events.subscribe` returns a function that when called, unsubscribes the entity from the event.
So we subscribe to the event in `async_added_to_hass` and unsubscribe in `async_will_remove_from_hass`.

`sensor.py`
```python {10-13,15-19} showLineNumbers
class MySensor(SensorEntity):
    """Representation of a sensor."""
    
    unsubscribe: Callable[[], None] | None = None

    def __init__(self, client: MyClient) -> None:
        """Initialize the sensor."""
        self.client = client
    
    async def async_added_to_hass(self) -> None:
        """Subscribe to the events."""
        await super().async_added_to_hass()
        self.unsubscribe = self.client.events.subscribe("my_event", self._handle_event)
    
    async def async_will_remove_from_hass(self) -> None:
        """Unsubscribe from the events."""
        if self.unsubscribe:
            self.unsubscribe()
        await super().async_will_remove_from_hass()
    
    async def _handle_event(self, event: Event) -> None:
        """Handle the event."""
        ...
        self.async_write_ha_state()
```

:::info
The above example can be simplified using lifecycle functions.
This saves the need to store the callback function in the entity.
```python showLineNumbers
    async def async_added_to_hass(self) -> None:
        """Subscribe to the events."""
        await super().async_added_to_hass()
        self.async_on_remove(
            self.client.events.subscribe("my_event", self._handle_event)
        )
```
:::

## Exceptions

There are no exceptions to this rule.
