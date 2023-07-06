## This is some notes of todos

The task I want to do is to add ability to have a chart of electricity consumption appear on my home assistant.

---

* Seems one needs to add a new sensor, namely a usage sensor, similar to how they are added here


```python

async def async_setup_entry(
    hass: HomeAssistant,
    entry: ConfigEntry,
    async_add_entities: AddEntitiesCallback,
) -> None:
    """Set up a config entry."""
    coordinator: AmberUpdateCoordinator = hass.data[DOMAIN][entry.entry_id]

    current: dict[str, CurrentInterval] = coordinator.data["current"]
    forecasts: dict[str, list[ForecastInterval]] = coordinator.data["forecasts"]

    entities: list = []
    for channel_type in current:
        description = SensorEntityDescription(
            key="current",
            name=f"{entry.title} - {friendly_channel_type(channel_type)} Price",
            native_unit_of_measurement=UNIT,
            state_class=SensorStateClass.MEASUREMENT,
            icon=ICONS[channel_type],
        )
        entities.append(AmberPriceSensor(coordinator, description, channel_type))
```

---

Seems `homeassistant/components/amberelectric/coordinator.py` has the logic for querying the API.

---

I think what I need to do next is figure out how to reset the last_reset value for the sensor