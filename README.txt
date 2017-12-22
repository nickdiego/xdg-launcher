How to build with SDK

1. make build directory
   $ mkdir build

2. do cmake
   $ cd build
   $ cmake ..

   You could find binary executable "runxdg" is built.
   "runxdg" should be installed into the directory
   on the target. (e.g. /usr/bin or /usr/local/bin)

3. Prepare configuration for RunXDG, 'runxdg.toml'
   See 'package/{runxdg|navi|hvac}/runxdg.toml for detail.

   'role', 'method', 'path' is mondatory.

   Only "POSIX" as 'method' works, others not tested so far.

   'role' should be the same which defined in layers.json of WindowManager.

   e.g. followings are predefined role by default
   "role": "MediaPlayer|Radio|Phone|Navigation|HVAC|Settings|Dashboard|POI|Mixer"

   In order to pass port number and token which are given from
   AGL application framework binder to XDG application,
   you can use special string '@port@' and '@token@'.

   For 'params', you can use special string '@port@' and '@token@"
   to pass them to target XDG application
   which are from AGL application framework binder.

   e.g. params = [ --port=@port@ --secret=@token@ ]

3. Prepare config.xml for widget

   <content> should be follow.
     <content src="bin/runxdg" type="application/vnd.agl.native"/>

   following <feature> is mandatory.
     <feature name="urn:AGL:widget:required-api">
       <param name="homescreen" value="ws" />
       <param name="windowmanager" value="ws" />
     </feature>

4. Make widgets
   $ make widget

   The following wgt would be made.
   - runxdg.wgt     XDG Launcher
   - navi.wgt       for test, XDG Launcher installed as Navigation
   - hvac.wgt       for test, XDG Lanncher installed as HVAC

