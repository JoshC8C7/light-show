# **Tesla Light Show xLights Guide**

Welcome to the Tesla Light Show xLights guide! You can create and run your own light shows on Tesla vehicles.

<img src="/images/xlights_overview.png?raw=true" width="1000" />

## Running a custom show on a vehicle
A custom show can be run on a supported vehicle by loading it via a USB flash drive. Create and share your shows with others! A single show can be shared and run on any supported vehicle; they are not model-specific. The sequence data is stored in a .fseq file and the music comes from your choice of .mp3 or .wav.
### Download a Show
Multiple light show repositories can be found online. A screenshot from [TeslaLightShare.io](https://teslalightshare.io/) is below.
## Light Show Sequence Validator Script
A Python validator script is provided to help check if your custom light show sequence meets these limitations, without needing a Tesla vehicle.

For usage, run:
```
python validator.py lightshow.fseq
```

Expected output looks like:
```
> python validator.py lightshow.fseq
Found 2247 frames, step time of 20 ms for a total duration of 0:00:44.940000.
Used 16.45% of the available memory
```

## Boolean Light Channels
Most lights available on the vehicle can only turn on or off instantly, which corresponds to 0% or 100% brightness of an 'Effect' in xLights.
- For off, use blank space in the xLights timeline
- For on, place *Turn On; Instant* (hotkey: 'F' on your keyboard)

The minimum on-time for boolean light channels to produce light is 15ms, although human eyes will have a hard time seeing anything this short! Minimum on-time or off-time of 100ms generally makes the show more pleasing to look at.

In this example, the Left Front Fog turns on and off 3x:

<img src="/images/on_effect_example.png?raw=true" width="550" />

## <a name="ramping_lights"></a>Ramping Light Channels
Some channels can have a slow ramp in the intensity during turn-on or turn-off, to create graceful visual effects:
| Channel | Model S | Model X | Model 3/Y |
| --- | --- | --- | --- |
| Outer Main Beam  | Ramping | Ramping | Ramping on LED reflector headlights; <br> Boolean on LED projector headlights |
| Inner Main Beam | Ramping | Ramping | Ramping |
| Signature | Boolean | Boolean | Ramping |
| Channels 4-6 | Ramping | Ramping | Ramping |
| Front Turn | Boolean | Boolean | Ramping |

To command a light to turn on or off and follow a ramp profile, place the effect with the corresponding keyboard shortcut:

| Ramping Function | xLights Effect Brightness |
| ------ | ----------- |
| Turn off; Instant | *empty timeline* |
| Turn off; 500 ms | W |
| Turn off; 1000 ms | S |
| Turn off; 2000 ms | X |
| Turn on; 500 ms | E |
| Turn on; 1000 ms | D |
| Turn on; 2000 ms | C |
| Turn on; Instant | F |

The keyboard layout is designed to be easy to use. Note that the effect types are grouped into vertical keyboard rows and sorted by duration.

<img src="/images/keyboard_shortcuts.png?raw=true" width="800" />

### Other notes
- Ramping can only prolong the time it takes to fully turn a light on or turn off. It is not possible to command a steady-state brightness setpoint between 0% and 100% intensity.
- If an xLights effect is longer than the ramping duration, then the light will stay at 0% or 100% intensity after it finishes ramping.
- Ramping effects can end early or be reversed before completion, and the light will immediately start following the new profile that is commanded.
- To guarantee that a light reaches the 0% or 100% setpoint, the xLights effect must have a duration at least 50ms greater than the ramp duration. Conversely, to guarantee that a light does not fully reach the 0% or 100% setpoint, the xLights effect must have a duration at least 100ms less than the ramp duration.
- Ramping Channels 4-6 have some unique aspects compared to other lights in order to use them effectively - see notes in [Ramping Channels 4-6](#ramping_channels_4_6).

### Ramping light examples
- Left inner main beam, *Turn on; 2000 ms*, effect duration 1s: causes the light to ramp from 0% to 50% intensity over 1s, then instantly turn off (because of the empty timeline).

    <img src="/images/ramp_example_1.png?raw=true" height="180" />

- Left inner main beam, *Turn on; 2000 ms*, effect duration 4s: causes the light to ramp from 0% to 100% intensity over 2s, then stay at 100% intensity for 2s, then instantly turn off after 4s.

    <img src="/images/ramp_example_2.png?raw=true" height="180" />

- Left inner main beam, *Turn on; 2000 ms* for 2.06s, *Turn off; 2000 ms* for 1.9s, *Turn on; 2000 ms* for 2.06s: causes light to ramp to 100% intensity, then down close to, but not reaching, 0% intensity, then back up to 100% intensity, then instantly turn off.

    <img src="/images/ramp_example_3.png?raw=true" height="180" />

## <a name="light_locations"></a>Light Channel Locations
The following tables and images help show which channels are controlled on each car. Some vehicles have lights that do not exist, or have multiple lights driven by the same control output - see notes in [Light channel mapping details](#light_channel_mapping_details) for this information.
| Light Channel Name | Identifier in image - Model S/X | Identifier in image - Model 3/Y |
| --- | --- | --- |
| Outer Main Beam | 1 | 1 |
| Inner Main Beam | 2 | 2 |
| Signature | 3 | 3 |
| Channel 4 | 4 | 4-6 |
| Channel 5 | 5 | 4-6 |
| Channel 6 | 6 | 4-6 |
| Front Turn | 7 | 7 |
| Front Fog | 8 | 8 |
| Aux Park | 9 | 9 |
| Side Marker | 10 | 10 |

### Model 3/Y with LED reflector lamps
<img src="/images/3_headlights_reflector.png?raw=true" width="900"/><br>

## <a name="closures"></a>Closures channels
In custom xLights shows, the following closures can be commanded:
| Channel | Model S | Model X | Model 3/Y | Supports Dance? | Command Limit Per Show |
| --- | --- | --- | --- | --- | --- |
| Liftgate | Yes | Yes | Only vehicles with power liftgate | Yes | 6 |
| Mirrors | Yes | Yes | Yes | - | 20 |
| Charge Port | Yes | Yes | Yes | Yes | 3 |
| Windows | Yes | Yes | Yes | Yes | 6 |
| Door Handles | Yes | - | - | - | 20 |
| Front Doors | - | Yes | - | - | 6 |
| Falcon Doors | - | Yes | - | Yes | 6 |

To command a closure to move in a particular manner, place an effect with the following keyboard shortcuts:

| Closure Movement | xLights Effect Brightness |
| --- | --- |
| Idle | *empty timeline* |
| Open | Q |
| Dance | A |
| Close | Z |
| Stop | F |

### Definitions for closure movements:
- **Idle:** The closure will stop, unless the closure is in the middle of an Open or Close in which case that movement will finish first.
- **Open:** The closure will open and then stop once fully opened.
- **Dance:** Get your party on! The closure will oscillate between 2 predefined positions. For the charge port, Dance causes the charge port LED to flash in rainbow colors.
- **Close:** The closure will close and then stop once fully closed.
- **Stop:** The closure will immediately stop.

### Other notes
- It's recommended to avoid closing windows during the show so that music stays more audible. Music only plays from the cabin speakers during the show.
- Closures can only dance for a limited time before encountering thermal limits. This depends on multiple factors including ambient temperature, etc. If thermal limits are encountered, the given closure will stop moving until it cools down. Dancing for ~30s or less per show is recommended.

### Closures Command Limitations
- All closures have actuation limits listed in the table above. Only Open, Close, and Dance count towards the actuation limits. The limits are counted separately for each individual closure.
- With the exception of windows, closures will not honor Dance requests unless the respective closure is already in the open position. The show creator must account for this by adding a delay between Open and Dance requests. Refer to [Closure Movement Durations](#closure_movement_durations) for more information.

### Closures Command xLights Notes
- For Idle, Open, Close, and Stop, there is no minimum xLights effect duration in order for the command to take effect. For example, the following sequence has a liftgate open command with duration of only 1s ahead of the dance that comes later, and this is sufficient to open the liftgate all the way:

    <img src="/images/open_and_dance.png?raw=true" width="850" />


### <a name="closure_movement_durations"></a>Closure Movement Durations
| Closure Movement | Approximate Duration (s) |
| --- | --- |
| Open Liftgate | 14 |
| Close Liftgate | 4 |
| Open Front Doors | 22 |
| Close Front Doors | 3 |
| Open Falcon Doors | 20 |
| Close Falcon Doors | 8 |
| Windows | 4 |
| Mirrors, Door Handles, Charge Port | 2 |


## Tips for platform-agnostic light shows
### Light channel mapping recommendations
- Not all vehicles have all types of lights installed. When turning on/off lights in sync with key parts of the music's beat, try to use lights that are installed on all vehicle variants.
- Not all vehicles have individual control over every light. In these cases, multiple xLights channels are OR'd together to decide whether to turn on a given group of lights. These are outlined in the section below.
- For lights that are controlled by multiple OR'd channels, keep in mind that some shared off-time on **all** of the OR'd channels is required to cause an apparent flash of the light.
    - This example will cause Aux Park lights to turn on constantly on Model 3/Y:

        <img src="/images/ord_channel_not_ok.png?raw=true" width="900" />

    - This example will cause the Aux Park lights to flash on Model 3/Y (note the blank time between each box compared to the other example):

        <img src="/images/ord_channel_ok.png?raw=true" width="900" />

### <a name="light_channel_mapping_details"></a>Light channel mapping details
#### Side Markers and Aux Park
- On Model 3/Y, all aux park and side markers operate together. They will activate during ```(Left side marker || Left aux park || Right side marker || Right aux park)``` requests from xLights.

#### <a name="ramping_channels_4_6"></a>Ramping Channels 4-6

- For Model 3/Y:
    - All 3 of these channels are combined into a single output on the vehicle, see [light channel locations](#light_locations).
    - The on/off state is OR'd together between all 3 channels: ```(Channel 4 || Channel 5 || Channel 6)```.
    - The ramping duration is defined by the effect placed in xLights for Channel 4.
- Because channel 4 acts as the leader for setting ramping duration on all platforms, a ramping effect must sometimes be included on Channel 4 even when Channel 4 is not turned on.
    - In this example, each channel blinks 3x and ramps down after its last blink. Notice how an *Turn Off, 500ms* effect is added always on Channel 4, even when it's Channel 5 or Channel 6 that were turned on.

        <img src="/images/channel_4_6_example.png?raw=true" width="850"/>

    - In this example, Channel 4 & 6 ramp up and ramp down again. Notice how Channel 6 has the *Turn On; Instantly* effect while Channel 4 defines the effect to be *Turn On; 1000ms* for both channels.

        <img src="/images/channel_4_6_example_2.png?raw=true" width="850"/>



