---
title: "ASUS laptop changes refresh rate, which causes black screen"
excerpt: "Screen turns black; screen flickering"
date: 2023-09-16 01:30:00 AM UTC
date_last_modified:
categories:
  - Journal
tags: 
  - ASUS
published: true
---

My ASUS laptop's screen is sporadically turning black, and this has been happening for many months already. It also turns black when I unplug my external display (which is also an ASUS monitor) or when I unplug the power cord.

I learned that I am not the only one experiencing this: see [here](https://learn.microsoft.com/en-us/answers/questions/492261/refresh-rate-automatically-changes-when-unpluging) or [here](https://www.reddit.com/r/Asustuf/comments/vrr38f/how_to_turn_off_panel_power_saver_mode_permanently/).
And I learned that, in my case, this happens when the refresh rate is changed to anything higher than 60hz. 
 
<!-- 
 - ["Refresh rate Automatically changes when unpluging the charger]"(https://learn.microsoft.com/en-us/answers/questions/492261/refresh-rate-automatically-changes-when-unpluging)
 - ["How to turn off Panel Power Saver mode permanently?"](https://www.reddit.com/r/Asustuf/comments/vrr38f/how_to_turn_off_panel_power_saver_mode_permanently/) 
-->

Something in my machine is forcibly changing my refresh rate to 144hz any time of the day. I suspect that it is the Armoury Crate app from ASUS, because the "Panel Power Saver" option in that app is always turned on during startup even when I already turned it off in the previous session. And so I have to always remember to turn it off right after startup so I will not experience having my screen turning black. (Sometimes my screen turns black right after I click on the "Panel Power Saver" button to turn it off. Very annoying)

Because of this experience, I had memorised how to set the refresh rate from 144hz back to 60hz without seeing anything on my screen:

1. Press Windows button

2. Type "display" using the keyboard, then press Enter. 

   This selects "Duplicate or extend to a connected display". Then is opens the Settings window, and automatically navigates to System -> Display

   Wait for a few seconds to be sure that everything has loaded.

3. Pres Tab key 18 times if an external display is connected (14 times if no external display is connected)

   This navigates to the "Advanced display" setion.

4. Press enter.

   This opens the "Advanced display" window.

5. Press Tab key 4 times. Then Press Enter

   This navigates to the dropdown for "Choose a refresh rate", then opens it making it ready for selection.

6. Press the Move Down button on the keyboard two times

   This selects the "60 Hz" option. Now, the screen is not black anymore.

   And a dialog box is being displayed asking if you want to keep changes or revert changes.

7. Click on "Keep changes" button in the dialog box

My problem was that changing the refresh rate to 60hz does not always work. Sometimes, even after restarting my machine multiple times and redoing the steps mentioned above multiple times, it does not work.

After many months of annoyance this has caused me, I think I have found the settings which makes this problem go away:

1. Using the Intel Graphics Command Center, switch off Panel Self Refresh for both "On Battery" and "Plugged In" settings

   Go to System -> Power

   ![Intel Graphics Command Center - Panel Self Refresh](/assets/images/2023/2023-09-16-intel-graphics-command-center-panel-self-refresh.png)

2. (might be optional) Using the Intel Graphics Command Center, switch off Adaptive Sync

   Go to Display -> Global Settings

   (Might be an optional step because this will be turned on in step 6)

3. Uninstall Armory Crate using the uninstall tool from the [Product Support for Armoury Crate page](https://www.asus.com/supportonly/armoury%20crate/helpdesk_download/)

   I learned that [I do not need Armory Crate](https://www.reddit.com/r/ASUS/comments/pq23y3/do_i_need_armoury_crate/). I'm also not into computer games, so I don't need special control over fans, and GPU and related stuffs.

4. Uninstall RefreshRateService using the "Add or remove programs" in Control Panel of Windows 11

   I accidentally found this in the Services app in Windows, so I uninstalled it. I'm not sure if this step is optional or not.

5. Uninstall ["Asus Smart Display Control" driver](https://www.quora.com/I-am-just-using-a-new-Asus-TUF-A17-but-when-I-unplug-the-charger-the-refresh-rate-changes-from-144z-to-60-Hz-How-do-I-keep-the-display-144Hz-all-the-time/answer/Andrew-12720)

6. Using the Intel Graphics Command Center, switch on Adaptive Sync

   ![Intel Graphics Command Center - Adaptive Sync](/assets/images/2023/2023-09-16-intel-graphics-command-center-adaptive-sync.png)

I'm hoping that ASUS will fix this issue soon.
