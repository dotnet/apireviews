# Xamarin Essentials

Status: **Needs more work** | 
[API Ref](Xamarin.Essentials.md) |
[Video](https://www.youtube.com/watch?v=MuID_T2I7ds)

## Notes

* Standardized API for typical mobile APIs (sensors)
    - Targets .NET Standard, which throws
    - Provides implementations for iOS, Android, and UWP
* They also provide additional APIs in the platform-specific implementation
* Most consumers are .NET Standard 2.0
    - Some use multi-targeting for apps would be low
    - Apps use Shared Projects
* Metrics are normalized so that things are consistent
    - Ideally, the unit should be clear from the property names

## Capability APIs

* Should the API generally model the case of multiple devices
    - The upside is that the device doesn't have a feature of natural flow
    - The downside is that it complicates the API for the 80% scenario (mobile
      sensors are usually in the 0 or 1 category, such as barometers or
      thermometers).
* `IsSupported` vs. `IsAvailable`
    - "Device doesn't have a capability" and "library doesn't have an
      implementation" seem like the same case and probably should be modeled
      the same way.
    - "User hasn't given permission" probably should be modelled differently as
      it seems the application might want to prompt the user to grant it.
* Capability APIs should never throw

## Static

* All types are `static`. This might be problematic for APIs that track state,
  such as `Barometer`. 
* Static event handlers have the problem that there isn't a good time to
  register them. My instance might not have called `Start()` yet, but somebody
  else did, so I start receiving events even though I'm not ready yet.
* Static event handlers are prone to keeping objects alive forever. You might
  want to look into `WeakReference` to store the handler and clean the list when
  you're raising the event. Alternatively, you should at least make sure your
  samples use well-known hooks and show how to unsubscribe, e.g. by using
  Xamarin Form's `Page.Closed` event.
