Here's a file needed to compile vim (64 bit) on VS.

I didn't have a copy of the license for VS2013, but if you need a newer version of Win32.Mak here are the
differences in this repository's copy and v7.1A\Include\Win32.Mak.

  # Win32.Mak - Win32 application master NMAKE definitions file for the
- #     Microsoft Plaform SDK for Win32 and Win64 programming samples
+ #     Microsoft Windows SDK programming samples
  #           Copyright (C) Microsoft Corporation

  #
- # Define APPVER = [ 4.0 | 5.0 | 5.01 | 5.02 | 6.0] prior to including win32.mak to get
+ # Define APPVER = [ 4.0 | 5.0 | 5.01 | 5.02 | 6.0 | 6.1] prior to including win32.mak to get
  #  build time checking for version dependencies and to mark the executable

  #
- # Define _WIN32_IE = [ 0x0300 | 0x0400 | 0x0500 | 0x0600] prior to including win32.mak to
+ # Define _WIN32_IE = [ 0x0300 | 0x0400 | 0x0500 | 0x0600 | 0x0700 | 0x0800] prior to including win32.mak to
  #  get compile and link flags for building applications and components to

  # For Call Attributed Profiling Info   nmake profile=1
- # For COLE 32->64-bit Porting Tool     nmake COLE_64=1
  #

  #
- # Note: to use the COLE_64 option, you need to set your build environment
- #       for 64-bit compilations.
- #
  # Note: TUNE and PROFILE do nothing for 64bit compilation

  !IFNDEF APPVER
  APPVER = 5.0
  !ENDIF

+ !IF "$(APPVER)" != "6.1"
  !IF "$(APPVER)" != "6.0"
  !IF "$(APPVER)" != "5.02"
  !IF "$(APPVER)" != "5.01"
  !IF "$(APPVER)" != "5.0"
  !IF "$(APPVER)" != "4.0"
- !ERROR Must specify APPVER environment variable (4.0, 5.0, 5.01, 5.02, 6.0)
+ !ERROR Must specify APPVER environment variable (4.0, 5.0, 5.01, 5.02, 6.0, 6.1)
+ !ENDIF
  !ENDIF
  !ENDIF
  !ENDIF
  !ENDIF
  !ENDIF

+ !IF "$(APPVER)" =="6.1"
+ !IFNDEF _WIN32_IE
+ _WIN32_IE = 0x0800
+ !ENDIF # _WIN32_IE
+ !ENDIF # APPVER == 6.1
+

  !IF "$(APPVER)" =="6.0"

  # -------------------------------------------------------------------------
  # Build tool declarations common to all platforms
  #    Check to see if Cole Porter is used, otherwise use C/C++ compiler
  # -------------------------------------------------------------------------

- !IFDEF COLE_64
- cc     = Port64
- link   = Cole & Rem no link when using Cole
- implib = Rem no lib when using Cole since the Cole .Objs are text
- !ELSE
  cc     = cl
  link   = link
  implib = lib
- !ENDIF

  midl   = midl

  !ELSEIF "$(CPU)" == "IA64"
- cflags = $(ccommon) -D_IA64_=1 -DWIN64 -D_WIN64  -DWIN32 -D_WIN32 /FIPRE64PRA.H
- cflags = $(cflags) -Wp64 -W4
+ cflags = $(ccommon) -D_IA64_=1 -DWIN64 -D_WIN64  -DWIN32 -D_WIN32
+ cflags = $(cflags) -W4
  scall  =

  !ELSEIF "$(CPU)" == "AMD64"
- cflags = $(ccommon) -D_AMD64_=1 -DWIN64 -D_WIN64  -DWIN32 -D_WIN32 /FIPRE64PRA.H
- cflags = $(cflags) -Wp64 -W4
+ cflags = $(ccommon) -D_AMD64_=1 -DWIN64 -D_WIN64  -DWIN32 -D_WIN32
+ cflags = $(cflags) -W4
  scall  =

  NMAKE_WINVER = 0x0600
+ !ELSEIF "$(APPVER)" == "6.1"
+ NMAKE_WINVER = 0x0601
  !ENDIF

  !ELSE IFDEF PROFILE
- cdebug = -Gh -Zd -Ox -DNDEBUG
+ cdebug = -Gh -Ox -DNDEBUG
  !ELSE IFDEF TUNE
- cdebug = -Gh -Zd -Ox -DNDEBUG
+ cdebug = -Gh -Ox -DNDEBUG
  !ELSE

  MIDL_OPTIMIZATION=-target NT60
+ !ELSEIF "$(APPVER)" == "6.1"
+ MIDL_OPTIMIZATION=-target NT61
  !ELSEIF "$(APPVER)" == "5.01"

  # Set the Output Directory
- !IF ("$(APPVER)" == "6.0")
- OUTDIR=LH
+ !IF ("$(APPVER)" == "6.1")
+ OUTDIR=WIN7
+ !ELSEIF ("$(APPVER)" == "6.0") 
+ OUTDIR=Vista
  !ELSEIF "$(APPVER)" == "5.0"
