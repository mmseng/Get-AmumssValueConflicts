# ALFCC
ALFCC stands for AMUMSS Lua File Conflict Checker. It is a tool intended to be used in conjunction with [AMUMSS](https://github.com/HolterPhylo/AMUMSS), to check for value-level conflicts in Lua files used by No Man's Sky mods.  

AMUMSS already identifies conflicts at the file level. However in order to resolve those conflicts, you must either troll through the individual Lua files looking for very specific values that are being changed (which is complicated by the fact that Lua files can be formatted in any number of ways), or run them through AMUMSS, potentially multiple times, and compare the resulting MBIN/EXML files (again, a time-consuming and painstaking process), or just hope that the mods don't conflict at the value-level.  

I created ALFCC because I wanted a faster and easier way to know, with certainty, whether the Lua files for any given mods are actually attempting to change the same exact values.  

Realistically, if you're smart with the kinds of mods you use together, you can already be pretty sure that they aren't conflicting at the value-level, even if they conflict at the file level. It's very unlikely that two mods will attempt to modify the same exact values, unless those mods are doing very similar things, in which case you probably shouldn't be using them together. However it's certainly not impossible for conflicts to happen, and it can be useful or interesting to compare the value-level changes between two similar mods.  

As such, is a fairly niche tool (even moreso than AMUMSS itself), so ALFCC is mostly just an extra tool in the toolbelt of mod developers. But it can be useful in some instances for mod users as well.  

# Behavior
ALFCC works by taking the names of specific Lua files (or interpreting them from AMUMSS logging output), executing those Lua files to read in the actual, final values that will be changed, and comparing them. Just reading the raw content of the Lua files is not sufficient because Lua file are just scripts which eventually build a Lua table variable (`NMS_MOD_DEFINITION_CONTAINER`) which contains the necessary data. Executing the Lua files first and _then_ reading the variable ensures that we ingest the final, effective changes that the mod wants to perform.  

# Validation
Because ALFCC must read the `NMS_MOD_DEFINITION_CONTAINER` table data produced by the mod Lua files, it also does some basic validation of this data against the bare minimum specifications of the required table syntax. This validation is relatively simplistic, but can still be a useful tool. ALFCC can be run in validation-only mode to make use of this in a standalone fashion. The validation code could be easily extended as well.  

# WIP
THIS TOOL IS CURRENTLY A WORK IN PROGRESS IN THE ALPHA STAGES! DO NOT USE IT YET!  

# Usage
WIP

# Parameters

## Primary parameters

### AmumssDir \<string\>
Optional string.  
The full path to your AMUMSS install directory.  
Omit trailing backslash.  
Default is `S:\AMUMSS\install`, just because that is the ALFCC author's AMUMSS directory.  
You will probably want to customize this directly in the script file, rather than provide it as a parameter every time.  

### LuaFilePaths \<string[]\>
Optional string array.  
The full paths to (ideally two or more) mod Lua files.  
Omitting this parameter causes ALFCC to instead parse `$AmumssDir\REPORT.lua` and use the conflicting files reported by AMUMSS as the files to compare.  
When using `-LuaFilePaths`, every given file will be compared to every other given file. Keep in mind that this means that every additional file given creates exponentially more work for ALFCC to do.

## Advanced parameters  

### ReportLuaRelativeFilePath \<string\>
Optional string.  
The path of AMUMSS' `REPORT.lua` file, relative to `-AmumssDir`.  
This should basically always just be the default of `REPORT.lua`.  
Just provided in case you want to test using copies or backups of `REPORT.lua` that are named differently or stored in other locations.  

### ValidateOnly
Optional switch.  
If specified, ALFCC skips comparing the Lua files and just does the initial validation of each file.  

### PassThru
Optional switch.  
If specified, a PowerShell object will be returned.  
The object will contain basically all of the information that was gathered and used by ALFCC during execution, so that it can be reviewed manually, or further acted/automated upon.  

I can't be bothered to document the anatomy of the object at the moment, but it's pretty straightforward.  

### ConflictBlockRegex \<string\>
Optional string.  
The regex string used to identify the the lines in `REPORT.lua` which contain information about which MBIN and Lua files are conflicting.  
Don't change this unless you know exactly what you're doing.  

### ConflictLuaRegex \<string\>
Optional string.  
The regex string used to further identify the conflicting Lua files from the lines identified by `-ConflictBlockRegex`.  
Don't change this unless you know exactly what you're doing.  

### LuaTableJsonScriptPath \<string\>
Optional string.  
The custom Lua script which is used to execute the conflicting mod Lua files.  

## Logging parameters

### Quiet
WIP

### Log
WIP

### LogRelativePath \<string\>
WIP

### LogFileName \<string\>
WIP

### LogFileTimestampFormat \<string\>
WIP

### LogLineTimestampFormat \<string\>
WIP

### Indent \<string\>
WIP

# Notes
- By mmseng. See my other projects here: https://github.com/mmseng/code-compendium-personal.
