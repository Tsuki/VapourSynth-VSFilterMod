# Aegisub #

Copy VSFilterMod.dll to C:\Program Files\Aegisub\csri directory, than start Aegisub, open Options dialog, select Video tab and change Subtitle provider to csri/vsfiltermod\_textsub.

**Important Note.** Aegisub doesn't know about new override tags so it will delete or replace them with known ones when you will try to use Visual Typesetting tools.

# Avisynth #

Copy VSFilterMod.dll to default Avisynth plugins directory (C:\Program Files\Avisynth 2.5\plugins) or load it with LoadPlugin() function. Mod provides similar to VSFilter functions but with "Mod" suffix e.g. TextSubMod().

# DirectShow #

To use VSFilterMod with video players first of all you should unregister installed version of VSFilter. Find where it's located on your disc and execute "regsvr32 /u Path\to\VSFilter.dll". Then place VSFilterMod.dll to some suitable for you place and execute "regsvr32 Path\to\VSFilterMod.dll".

# VirtualDub #

Copy VSFilterMod.dll to VirtualDub plugins directory and change its extension to vdf (it should be VSFilterMod.vdf now). Then select TextSubMod from filters and work as usual.