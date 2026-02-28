# Kometa Overlay for Movies and TV Shows

> [!NOTE]
> All credit goes to jmxd for creating the base presets of the overlays.
>
> This fork combines several additions into one

## Adjustments by olli991

- extended the resolutions with `720P` and `SD` (for sub 720p stuff)
- added two more colors from [toovirals fork](https://github.com/tooviral/Kometa-custom-overlays)
- added grey color and `-` for 10.0 ratings
- added text for status of TV Shows (Airing, Ended, Canceled, Returning)
- added flags from [Craftwork2720](https://github.com/Craftwork2720/pmm-rating-and-subtitle-flag-for-movies) inspired by [gravelfreemans fork](https://github.com/gravelfreeman/kometa-posters?tab=readme-ov-file) but changed it into two language support and display on the left and right of the rating. Resized the flags to fit it.
- added additional audio codecs from [shivam183s fork](https://github.com/shivam183/Kometa/tree/local-main). Adjusted some of the images and implementeded it into the current regex. Also added some rare format detection like DTS-ES or DTS-HRA
- extended the whole detection and images for combinations of DV, HDR and HDR+ with old audio codecs
- added the generation of overlays for season posters and episodes

Changes are editied into sections below.

## audience_rating.yml

This file will create an overlay showing the current audience rating value in PLEX. Ratings of 9 or higher are dark green, 7.5 or higher are light green, 6.5 or higher are yellow,  5 and or higher are orange and anything lower will be red.

## media_info.yml

![DV Atmos](https://i.imgur.com/CLnpX5j.png)

This file can be used to create an overlay containing media info characteristics of your media. It comes with custom designed images for all combinations of supported formats. This way they can be displayed as one without forced spacing or repeating elements.

Below is an example of what this builder will create.

> [!IMPORTANT]
> For this template to work correctly you MUST use the TRaSH naming convention for your file names.
>
> [https://trash-guides.info/Radarr/Radarr-recommended-naming-scheme/#plex](https://trash-guides.info/Radarr/Radarr-recommended-naming-scheme/#plex)

Movies:<br>
<img width="922" height="384" alt="image" src="https://github.com/user-attachments/assets/7c57d138-5827-4076-846f-f6c5da6fa61a" />
<br>
<br>
Series:<br>
<img width="917" height="383" alt="image" src="https://github.com/user-attachments/assets/f433fb65-f00e-47c7-8b26-ba4fdefaebdd" />
<br>
Seasons:<br>
<img width="466" height="389" alt="image" src="https://github.com/user-attachments/assets/55db7320-abe2-455f-84b2-e56597d0ecc8" />
<br>
Episodes:<br>
<img width="459" height="316" alt="image" src="https://github.com/user-attachments/assets/8b9ab32d-3177-4e29-8ca0-4301a956700f" />

Original
![Overlay Example](https://i.imgur.com/xgEv2Oe.png)

## Currently supported overlay images in all combinations

| Resolutions | Editions            | Video Formats         | Audio Formats      |
| -           | -                   | -                     | -                  |
| SD (SD)     | Anniversary Edition | HDR                   | MP3                |
| 480p (SD)   | Collector's Edition | HDR10+                | AAC                |
| 576p (SD)   | Director's Cut      | Dolby Vision          | OPUS               |
| 720p (720P) | Extended Cut        | Dolby Vision + HDR    | FLAC               |
| 1080p (HD)  | Extended Edition    | Dolby Vision + HDR10+ | Dolby Digital      |
| 2160p (4K)  | Final Cut           |                       | Dolby Digital Plus |
|             | IMAX / (Enhanced)   |                       | DTS                |
|             | Minus Color         |                       | DTS-HD             |
|             | Open Matte          |                       | DTS-X              |
|             | Special Edition     |                       | Dolby Atmos        |
|             | Unrated Edition     |                       | Dolby TrueHD       |
|             | ... and more        |                       | Dolby TrueHD Atmos |

> [!NOTE]
> Dolby Vision with HDR/HDR10+ fallback support is correctly detected and matched separately from exclusive DV, but only Dolby Vision will be visibly shown for those files. See examples at the bottom for an alternative option.

### Available template variables

----------

#### Exclusive use

`video_only` `audio_only` `resolution_only` `edition_only`

Set to `true` to only apply this specific type of overlay.

#### Disable specific overlays

`use_<<type>>` e.g. `use_audio`

Set to `false` to disable this type of overlay (combined, video, audio, resolution, edition)

> [!IMPORTANT]
> To disable audio or video you must set `use_combined` to `false` as well.

`use_gradient_top` `use_gradient_bottom`

Set to `false` to disable the backdrops

Any standard template variables are available as well, such as `builder_level`

## Examples

Use it as intended

```yml
overlay_files:
  - file: config/overlays/media_info.yml        # modified with SD support
  - file: config/overlays/audience_rating.yml   # modified with the two extra colors
```

Using additional tv shows status overlay

```yml
overlay_files:
  # Adding tv show status overlay and colors
  - default: status            
    template_variables:
      text_airing: "LÄUFT AKTUELL"
      text_returning: "KOMMT ZURÜCK"
      text_canceled: "ABGESETZT"
      text_ended: "BEENDET"
      horizontal_align: center
      vertical_align: top
      vertical_offset: 30
      back_color: "#FFFFFF00"
      font: config/overlays/fonts/AvenirNextLTPro-Bold.ttf
      font_size: 60
      font_color_airing: "#2D8B2D"
      font_color_returning: "#5CB85C"
      font_color_canceled: "#E23133"
      font_color_ended: "#FFFFFF"
```

Using additional language flags besides the audience rating on movies and tv shows

```yml
overlay_files:
  # Adding langugage flags besides the rating overlay
  - default: languages            
    template_variables:
      languages:
        - de                  # First language - left to the rating overlay
      use_subtitles: false
      file: config/overlays/flags/<<key>>.png
      hide_text: true
      back_width: 40
      back_height: 80
      back_color: "#FFFFFF00"
      horizontal_offset: 179
      horizontal_align: right
      vertical_offset: 50
      vertical_align: bottom
  - default: languages
    template_variables:
      languages:
        - en                  # Second language - right to the rating overlay
      use_subtitles: false
      file: config/overlays/flags/mirrored/<<key>>.png
      hide_text: true
      back_width: 40
      back_height: 80
      back_color: "#FFFFFF00"
      horizontal_offset: 22
      horizontal_align: right
      vertical_offset: 50
      vertical_align: bottom 
```

Also applying overlays to tv show seasons and episodes

```yml
overlay_files:
  # Also appling the same overlays to episodes
  - file: /config/overlays/media_info.yml 
    template_variables:
      builder_level: episode
  - file: /config/overlays/audience_rating.yml 
    template_variables:
      builder_level: episode
  
  # Also appling the same overlays to seasons
  - file: /config/overlays/media_info.yml 
    template_variables:
      builder_level: season     
```

Also applying language flags to tv show episode ratings

```yml
overlay_files:
# Also adding the langugage flags besides the rating overlay in episodes
  - default: languages            
    template_variables:
      builder_level: episode
      languages:
        - de                  # First language - left to the rating overlay
      use_subtitles: false
      file: config/overlays/flags/<<key>>.png
      hide_text: true
      back_width: 40
      back_height: 80
      back_color: "#FFFFFF00"
      horizontal_offset: 179
      horizontal_align: right
      vertical_offset: 50
      vertical_align: bottom
  - default: languages
    template_variables:
      builder_level: episode
      languages:
        - en                  # Second language - right to the rating overlay
      use_subtitles: false
      file: config/overlays/flags/mirrored/<<key>>.png
      hide_text: true
      back_width: 40
      back_height: 80
      back_color: "#FFFFFF00"
      horizontal_offset: 22
      horizontal_align: right
      vertical_offset: 50
      vertical_align: bottom   
```

Disable audio

```yml
overlay_files:
  - file: config/overlays/media_info.yml
    template_variables:
      use_combined: false
      use_audio: false
  - file: config/overlays/audience_rating.yml
```

Only apply video formats and change the images for these two overlays to also show the fallback format. Then, only show audio, and move it to the right

```yml
overlay_files:
  - file: config/overlays/media_info.yml
    template_variables:
      builder_level: episode
      video_only: true
      use_gradient_top: false
      file_DV-HDR: config/overlays/images/media_info/codec/DV-HDR_extended.png
      file_DV-Plus: config/overlays/images/media_info/codec/DV-Plus_extended.png
  - file: config/overlays/media_info.yml
    template_variables:
      builder_level: episode
      audio_only: true
      use_gradient_top: false
      use_gradient_bottom: false
      horizontal_align: right
```

> [!IMPORTANT]
> When running the overlay file multiple times on the same library like the above example, make sure to disable both gradients on subsequent uses or it will apply them again.

<br />

### ☕ [buymeacoffee.com/jmxd](https://buymeacoffee.com/jmxd)

<br /><br />

<img align="left" height="50px" src="https://i.imgur.com/YzGqu5U.png">
<img align="right" height="50px" src="https://i.imgur.com/Ip6nj8m.png">
