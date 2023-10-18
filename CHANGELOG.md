# `com.peoplefun.platform-tech.core` Changelog

## 0.7.0
--------------------------------
### feat
- added adHealth event to track the loading and display of advertisements in the game. 

## 0.6.0
--------------------------------
### feat
- added screenView event to collect screen changes.  This must be instrumented in the game using analytics.ScreenView.Track on all transitions
- added deviceInfo event to collect more data about the physical device the app is running on.  Automatically collected and sent at the beginning of the session.
- added appPerformance event to collect FPS and memory information from the game. A heartbeat is collected automatically (unless disabled) but addition sections can be profiled using AppPerformance.TrackBegin and then TrackComplete on the returned handle.
- added tokenExpiration param to DeviceLogin. Games can now request token a expiration that are less than the server default (24 hours).
- added token expiration debug menus to pt debug package
- added finalBalances to resourceTransaction events (no client changes required). finalBalances = balances + sources + sinks (negative)
### fix
- Changed network issues in Analytics to be warnings since they are expected at certain points.
- Experiment tests fixes and improvements
- Debug Enroll via Debug Menu now retains existing experiment enrollments and only overrides the requested enrollment treatment group (intended approach)

## 0.5.2
--------------------------------
### fix
- catching exception from deserialization of an error message to prevent lockup of analytics after a jwt expiration
- removing unnecessary async waits in the identity update function which were causing perf issues

## 0.5.1
--------------------------------
### test
- added coverage for storage namespace feat

### bug
- fix `[pt.core][error] Account not found. Please create a new account` for account upgrades to new app network and app env. Storage is now namespaced, but defaults to no namespace for current config (staging network + appenv = null)

## 0.5.0
--------------------------------
### test
- added coverage for analytics and identity integrations (huge improvements also)
- improved all tests and made test multi-env / network (appEnv + appNetwork)

### feat
- added `ExperimentSnapshot` event (auto reported)
- added `AppEnv` (see https://peoplefun.atlassian.net/wiki/spaces/PT/pages/2690187281/App+Getting+Started)
- extracted `IIdentityService` jwt features into `IJwtService`. Usable via `app.Jwt.PlayerId` and `app.Jwt.SyntheticDeviceId`
- added `AutoDeviceLogin` flag to network config to allow ability to disable auto device login before creating PT App

### bug
- fix unauthorized jwt will flag jwt tokens for network v2 requests (not analytics atm)
- fix minor bug pterror not deserializing correctly after 0.2.0

## 0.4.0
--------------------------------
### feat
- Moved language and country into helpers
- Added `metadata` common field on analytics custom events; optional param `object metadata = null` to all `Track` event methods
- Added `WaitForAccessToken` to `IdentityService` to all non-device login endpoints that require a jwt token
- Added `PlayerCreatedAt` to `IdentityService`
- Added `IStorageService:DeleteKeys(IEnumerable<string> keys)`
- Added `PtStorageKeys` to centralized Platform Tech Storage Keys
- Updated editor only device id's prefix approach
- Added `ExperimentsSnapshot` auto-tracked event (Disk, Enroll, DebugEnroll)

### fix
- Fix for enrollments when serialized to match backend format (debug only)
- Fix `System.ArgumentException => Region name is not supported. Parameter name: name` https://peoplefun.sentry.io/issues/4297034225/events/70c8963c65474385a14528c7a0a6b6c9/
- Fix 

## 0.3.0
--------------------------------
### fix
- moved Debug Panels into its own package
- silenced two warnings

## 0.2.7
--------------------------------
### fix
- fix for debug menu experiment treatments key to use treatment id instead of weight

## 0.2.6
--------------------------------
### fix
- fix for debug menu enrollments using experiment id as treatment id

## 0.2.5
--------------------------------
### fix
- added "refresh" button to experiments debug also for debug race condition / cancel

## 0.2.4
--------------------------------
### feat
- added forget identity debug button
- added refresh enrollments debug button
- remove post body requirement (can be empty)
- added deleteKey option to storage

### fix
- debug menu errors platform enrollment and debug enroll

## 0.2.3
--------------------------------
### fix
- fix ios deserialization issue with JSON.net JsonProperty field not working on device. reverted to basic `[Serializable]` instead of `[JsonProperty]`

## 0.2.2
--------------------------------
### fix
- fix android / editor precompile define missing _identityService reference

## 0.2.1
--------------------------------
### fix
- fix for metadata on clientAttribution event / private

## 0.2.0
--------------------------------
### feat
- (INTERNAL) Refactored Identity to use a new NetworkV2 service (Tasks). Analytics still using Network atm, but will be replaced in upcoming release.
- added Platform Tech Tasks now optionally use Cancellation Token (see below)
- added `Task<JwtAccessToken> WaitForAccessToken(CancellationToken? cancellationToken = null)` Task to `IIdentityService`
- added `Task<DeviceLoginResponse> DeviceLogin(CancellationToken? cancellationToken = null)` Task to `IIdentityService`
- created `IExperimentsService` and added to `PlatformTechApp` `Experiments` property
- added `Task<Enrollment[]> Enroll(enrollmentParams, cancelEnroll.Token)` Task to `IExperimentsService`
- added `Task<Enrollment[]> DebugEnroll(IEnumerable<Enrollment> enrollments, CancellationToken? cancellationToken = null)` Task to `IExperimentsService`
- added `Task<DebugExperiment[]> DebugList(CancellationToken? cancellationToken = null)` Task to `IExperimentsService`
- add basic Identity, Experiment, and Metadata Debug Menus - optional to be integrated into games
- add `DefaultWebRequestTimeoutMs` to `NetworkConfig` defaults to 3 secs (3000 ms). This will eventually replace Network `SendRequestTimeout` also.

## 0.1.1
--------------------------------
### fix
- TrackStart and TrackResume now take an IEnumerable for levelProgressData rather than an array (to match the other functions)

## 0.1.0
--------------------------------
### feat
- added server time service to coordinate an offset between the time the server reported on the last communication and the time on the client.  PlatformTechApp.ServerTime.GetEstimatedServerTime() will return the UTC time modified by the offset.
- added additional info to error logs
- added automatically tracked field `levelElapsedSec` to `gameLevel` standard event - time played during a level except when paused / backgrounded
- added `clientAttribution` standard event - used to report adjust attibution client side.  `PlatformTechApp.Analytics.Attribution.Track()` takes 4 strings (name, sdkVersion, network and campaign) and a collection of metadata that can be passed as an IDictionary, an ISerializable, or a json string.
- added config `StartupConfig.IdentityConfig`
- added callback event `IdentityConfig.AccessTokenChanged` - useful to initialize other dependencies on jwt access token
- added callback event `IdentityConfig.PlayerIdChanged` - useful to initialize other dependencies on playerId
- refactored MessageError helpers into `IErrorMessage`, `ClientErrorCodes`, and `ClientErrors`
- added `ServerErrorCodes`

### fix
- estimate server time when checking jwt expiration
- improved handling of a few potential null ref error cases
- modified GameLevel.Track functions to take an IEnumerable of KeyValuePairs rather than an Array. (for added flexibility).  This will require code changes if you are using the levelProgressData parameter.
- levelConfig passed with every level event if specified in the Start event.

## 0.0.13
--------------------------------
### fix
- changed the error handling for analytics calls

## 0.0.12
--------------------------------
### feat
- refactor of LevelEvent to make it easier to use with less parameters
- added clientMetadata dictionary for passing in pt-client version with identity login

### fix
- Only send app open and ready states one time per app launch
- do not resend event batches with schema errors

## 0.0.11
--------------------------------
### fix
- fix null issue with guid verification

## 0.0.10
--------------------------------
### feat
-   MVP for FastBlock CPI