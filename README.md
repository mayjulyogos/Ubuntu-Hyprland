# Ubuntu-Hyprland
This is how i set up my own UBUNTU laptop with Hyprland.

<br>

## Installation
- Install `Hyprland`
```bash
# Hyprland Installation
sudo apt update

sudo add-apt-repository universe && sudo apt update && sudo apt install hyprland

sudo apt install -y \
git \
build-essential \
gcc \
g++ \
cmake \
meson \
ninja-build \
pkg-config \
gettext \
gettext-base \
cpio \
jq \
wget \
curl \
pkg-config \
libgdk-pixbuf-2.0-dev \
libpango1.0-dev \
libgtk-4-dev \
libgtk4-layer-shell-dev \
libpoppler-glib-dev \
protobuf-compiler \
hyprland-dev \
libgles2-mesa-dev

# Graphics and redndering dependencies
sudo apt install -y \
libdrm-dev \
libegl-dev \
libegl1-mesa-dev \
libgles2-mesa-dev \
libgbm-dev \
libglvnd-dev \
glslang-tools \
libpixman-1-dev \
libdisplay-info-dev

# Wayland dependencies
sudo apt install -y \
wayland-protocols \
libwayland-dev \
libwayland-client0 \
libwayland-server0 \
libxkbcommon-dev \
libxkbcommon-x11-dev \
libxkbregistry-dev 

# X11 and XCB dependencies
sudo apt install -y \
libxcb1-dev \
libxcb-dri3-dev \
libxcb-composite0-dev \
libxcb-ewmh-dev \
libxcb-icccm4-dev \
libxcb-present-dev \
libxcb-render0-dev \
libxcb-render-util0-dev \
libxcb-res0-dev \
libxcb-xinput-dev \
libx11-dev \
libxfixes-dev \
libxrender-dev \
libxcomposite-dev \
libxcursor-dev

# Other Hyprland dependencies
sudo apt install -y \
libinput-dev \
libinput-bin \
libseat-dev \
libudev-dev \
libpango1.0-dev \
libcairo2-dev \
libfontconfig-dev \
libffi-dev \
libxml2-dev \
libtomlplusplus-dev \
libre2-dev

# Desktop Utilities
sudo apt install -y \
kitty \
pavucontrol \
network-manager \
blueman

# Hyprland-related utilities
sudo apt install -y \
hyprpaper \
hyprlock \
hypridle \
hyprpolkitagent \
xdg-desktop-portal \
xdg-desktop-portal-wlr \
hyprland-qtutils
```

<br>

---

<br>

## Key-Bindings
- Press `Win + R` to open `Terminal`
- Press `Win + C` to terminate an `Application`
- Press `Win + M` to exit `Hyprland`
- Press `Win + L` to perform `Lock Screen` ( Using `Hyprlock` with customized `key-bindings` )
- Press `Crtl + Space` to switch `Language Input` ( Using `Fctix 5` )
- Press `Win + Shift + S` to toggle screenshot ( Using `Grim` )
- Press `Win + Ctrl + S` to toggle `Hyprcapture` editor
- Press `Win + Shift + W` to toggle `Hyprcapture` recorder to record your `Window`
- Press `Win + Shift + F` to toggle `Hyprcapture` recorder to record your `Full Screen`

<br>

---

<br>

## Walker (App Launcher)
- Install `Elephant`
```bash
# Clone the repository
git clone https://github.com/abenz1267/elephant
cd elephant

# Build and install the main binary
cd cmd/elephant
go install elephant.go

# Create configuration directories
mkdir -p ~/.config/elephant/providers

# Build and install a provider (example: desktop applications)
cd ../../internal/providers/desktopapplications
go build -buildmode=plugin
cp desktopapplications.so ~/.config/elephant/providers/
```
- Install `Walker`
```bash
# Clone the repository
git clone https://github.com/abenz1267/walker.git
cd walker

# Build with Cargo
cargo build --release

# Run Walker
./target/release/walker
```
- Add `elephant` to your `hyprland.conf` file under `---- AUTOSTART ----`
```bash
# -------------------
# ---- AUTOSTART ----
# -------------------
exec-once = nm-applet
exec-once = /usr/lib/polkit-gnome/polkit-gnome-authentication-agent-1
exec-once = hyprpaper
exec-once = hyprpm reload
exec-once = sh -c elephant
exec-once = sh -c 'cd /home/ubuntu/walker && ./target/release/walker'
```
- Add `Walker` key-bindings to toggle the function
```bash
# ---------------------
# ---- KEYBINDINGS ----
# ---------------------
$mainMod = SUPER

bind = $mainMod, Q, exec, $terminal
bind = $mainMod, C, killactive
bind = $mainMod, M, exec, command -v hyprshutdown >/dev/null 2>&1 && hyprshutdown || hyprctl dispatch exit
bind = $mainMod, E, exec, $fileManager
bind = $mainMod, V, togglefloating
bind = $mainMod, P, pseudo
bind = $mainMod, J, togglesplit

# Move focus with mainMod + arrow keys
bind = $mainMod, left, movefocus, l
bind = $mainMod, right, movefocus, r
bind = $mainMod, up, movefocus, u
bind = $mainMod, down, movefocus, d

# Switch workspaces with mainMod + [0-9]
bind = $mainMod, 1, workspace, 1
bind = $mainMod, 2, workspace, 2
bind = $mainMod, 3, workspace, 3
bind = $mainMod, 4, workspace, 4
bind = $mainMod, 5, workspace, 5
bind = $mainMod, 6, workspace, 6
bind = $mainMod, 7, workspace, 7
bind = $mainMod, 8, workspace, 8
bind = $mainMod, 9, workspace, 9
bind = $mainMod, 0, workspace, 10

# Move active window to a workspace with mainMod + SHIFT + [0-9]
bind = $mainMod SHIFT, 1, movetoworkspace, 1
bind = $mainMod SHIFT, 2, movetoworkspace, 2
bind = $mainMod SHIFT, 3, movetoworkspace, 3
bind = $mainMod SHIFT, 4, movetoworkspace, 4
bind = $mainMod SHIFT, 5, movetoworkspace, 5
bind = $mainMod SHIFT, 6, movetoworkspace, 6
bind = $mainMod SHIFT, 7, movetoworkspace, 7
bind = $mainMod SHIFT, 8, movetoworkspace, 8
bind = $mainMod SHIFT, 9, movetoworkspace, 9
bind = $mainMod SHIFT, 0, movetoworkspace, 10

# Special workspace (scratchpad)
bind = $mainMod, S, togglespecialworkspace, magic
bind = $mainMod SHIFT, S, movetoworkspace, special:magic

# Scroll through existing workspaces with mainMod + scroll
bind = $mainMod, mouse_down, workspace, e+1
bind = $mainMod, mouse_up, workspace, e-1

# Move/resize windows with mainMod + LMB/RMB and dragging
bindm = $mainMod, mouse:272, movewindow
bindm = $mainMod, mouse:273, resizewindow

# Laptop multimedia keys for volume and LCD brightness
bindel = , XF86AudioRaiseVolume, exec, wpctl set-volume -l 1 @DEFAULT_AUDIO_SINK@ 5%+
bindel = , XF86AudioLowerVolume, exec, wpctl set-volume @DEFAULT_AUDIO_SINK@ 5%-
bindl = , XF86AudioMute, exec, wpctl set-mute @DEFAULT_AUDIO_SINK@ toggle
bindl = , XF86AudioMicMute, exec, wpctl set-mute @DEFAULT_AUDIO_SOURCE@ toggle
bindel = , XF86MonBrightnessUp, exec, brightnessctl -e4 -n2 set 5%+
bindel = , XF86MonBrightnessDown, exec, brightnessctl -e4 -n2 set 5%-

# Media control via playerctl
bindl = , XF86AudioNext, exec, playerctl next
bindl = , XF86AudioPause, exec, playerctl play-pause
bindl = , XF86AudioPlay, exec, playerctl play-pause
bindl = , XF86AudioPrev, exec, playerctl previous

# Walker launcher
bind = $mainMod, R, exec, sh -c 'cd /home/ubuntu/walker && ./target/release/walker'
```

<br>

---

<br>

## Hyprlock
- Installation
```bash
sudo apt install hyprlock
```
- Create `Hyprlock` config file
```bash
nano ~/.config/hypr/hyprlock.conf
```
- Paste these following lines into the config file
```bash
background {
    monitor =
    path = /home/arch/Pictures/annie-spratt-d8FjB1_8xHQ-unsplash.jpg
    color = rgba(0, 0, 0, 0.0)

    # Glass Blur Configuration
    blur_passes = 3           # More passes create a smoother, heavier glass effect
    blur_size = 8             # Higher size increases the blur radius
    noise = 0.015             # Adds a subtle grain texture common in frosted glass
    contrast = 1.0            # High contrast keeps colors vivid behind the glass
    brightness = 0.8          # Lower brightness mimics tinted or frosted glass
    vibrancy = 0.2            # Enhances colors pushing through the blur
    vibrancy_darkness = 0.0
}

# Time
label {
    monitor =
    text = $TIME
    color = rgb(205, 214, 244) # Text color
    font_size = 90
    font_family = JetBrainsMono Nerd Font Bold
    position = 0, 200
    halign = center
    valign = center
}

# Date
label {
    monitor =
    text = cmd[update:43200000] echo "$(date +"%A, %d %B")"
    color = rgb(1bac, 207, 246)
    font_size = 24
    font_family = JetBrainsMono Nerd Font
    position = 0, 100
    halign = center
    valign = center
}

# Password-Input
input-field {
    monitor =
    size = 250, 60
    outline_thickness = 3
    dots_size = 0.2
    dots_spacing = 0.2
    dots_center = true

    # Border color
    outer_color = rgb(137, 180, 250)
    inner_color = rgb(49, 50, 68)
    font_color = rgb(205, 214, 244)

    fade_on_empty = false
    placeholder_text = <i>Input Password...</i> # Prompt Text
    hide_input = false

    position = 0, -100
    halign = center
    valign = center
}
```
- Add `Hyprlock` key-bindings to your `hyprland.conf`
- Add these following lines under `---- KEYBINDINGS ----` 
```bash
-- Hyprlock
hl.bind(mainMod .. " + L", hl.dsp.exec_cmd("hyprlock"))
```

<br>

---

<br>

## Ghostty
- Installation
```bash
sudo apt install ghostty

# 1. Create the ghostty configuration folder
mkdir -p ~/.config/ghostty

# 2. Open and create the config file as your normal user
nano ~/.config/ghostty/config
```
- Configs
```bash
# --- Font ---
font-family = JetBrains Mono
font-size = 15
font-variation = wght=600

# --- Appearance & Window ---
foreground = #ffffff
background = #000000
selection-foreground = #000000
selection-background = #ffffff

# Liquid Glass Visuals
# background-opacity = 0.75
# background-blur-radius = 50

window-padding-x = 10
window-padding-y = 10
window-decoration = false
maximize = true

# --- Cursor ---
cursor-style = block
cursor-style-blink = true

# --- Mouse & Scroll ---
scrollback-limit = 100000000
mouse-hide-while-typing = true

# --- Keybindings ---
keybind = ctrl+d=new_split:right
keybind = ctrl+shift+d=new_split:down
keybind = ctrl+t=new_tab
keybind = ctrl+w=close_surface

# --- Enable xterm-256color ---
term = xterm-256color
```

<br>

---

<br>

## Wallpaper
- Hyprpaper
```bash
sudo apt install -y hyprpaper
```
### Configure hyprpaper config file
```bash
nano /home/ubuntu/.config/hypr/hyprpaper.conf
```
- Paste these following lines into the `hyprpaper.conf` file
- You may replace `/home/ubuntu/Pictures/wallpaper.jpg` with your own wallpaper path
```bash
preload = /home/ubuntu/Pictures/wallpaper.jpg

wallpaper {
    monitor = eDP-1
    path = /home/ubuntu/Pictures/wallpaper.jpg
}
```
- If you got two display you may need to add one more `wallpaper { ... }` with the monitor name changed
```bash
reload = /home/ubuntu/Pictures/wallpaper.jpg

wallpaper {
    monitor = eDP-1
    path = /home/ubuntu/Pictures/wallpaper.jpg
}

wallpaper {
    monitor = DP-1
    path = /home/ubuntu/Pictures/wallpaper.jpg
}
```

<br>

### Configure the Autostart part in hyprland.conf
```bash
nano /home/ubuntu/.config/hypr/hyprland.conf
```
- Replace the lines in your `Hyprland.lua` under `---- AUTOSTART ----` Section with these following lines
- By adding in `hl.exec_cmd("hyprpaper")`, `Hyprpaper` will be launched automatically each time you start up `Hyprland`
```bash
# -------------------
# ---- AUTOSTART ----
# -------------------
exec-once = nm-applet
exec-once = /usr/lib/polkit-gnome/polkit-gnome-authentication-agent-1
exec-once = hyprpaper
```
- Prese `Ctrl + O` to save and `Ctrl + X` to exit

<br>

### Fix the File Ownership
- Run this command to make sure your regular user account owns your configuration file
```bash
sudo chown ubuntu:ubuntu ~/.config/hypr/hyprland.conf
```
- Reboot to apply changes
```
systemctl reboot
```

<br>

---

<br>

## Kitty Configs
- Installation
```bash
sudo apt install kitty
```
- `Font size`, `Font Family`, `Scroll back Limits`, `Background Opacity` and `Background Color`
- Create `kitty.conf`
```bash
cd /home/ubuntu/.config

mkdir kitty
cd kitty

nano kitty.conf
```
- Paste these following lines into `kitty.conf`
```bash
font_size 14

font_family      JetBrainsMono Nerd Font
bold_font        auto
italic_font      auto
bold_italic_font auto

scrollback_lines 16384000

window_padding_width 10

confirm_os_window_close 0

background_opacity 0.5

background #000000



# Map CJK Unified Ideographs to your preferred Chinese font
symbol_map U+4E00-U+9FFF PingFang SC Bold
symbol_map U+3400-U+4DBF PingFang SC Bold
symbol_map U+20000-U+2A6DF PingFang SC Bold
```

<br>

---

<br>

## Brightness
- Install brightnessctl
```bash
sudo apt install -y brightnessctl
```
- Reload Hyprland
```bash
hyprctl reload
```

<br>

---

<br>

## Graphical Volume Control
```bash
sudo apt install -y pavucontrol
```

<br>

---

<br>

# Battery Info
- Ensure `upower` Daemon is Running
```bash
# Check UPower Status
systemctl status upower.service

# Enable and Start UPower
sudo systemctl enable --now upower.service
```

<br>

---

<br>

## Screenshot function
### Grim
- Install Dependencies
```bash
sudo apt install -y grim slurp wl-clipboard libnotify
```
- Paste these following lines into `Hyprland.conf` file under `---- KEYBINDINGS ----` section
```bash
# Screenshots
bind = SUPER SHIFT, S, exec, grim -g "$(slurp)" - | wl-copy
```

<br>

---

<br>

## Final Config
```bash
# #######################################################################################
# AUTOGENERATED HYPRLAND CONFIG.
# TRANSLATED FROM hyprland.lua
# #######################################################################################

# ------------------
# ---- MONITORS ----
# ------------------
monitor = eDP-1, 1920x1080@60, auto, 1
monitor = HDMI-A-1, preferred, auto-right, 1
monitor = DP-1, 2560x1440@144, auto-right, 1, transform, 2

# Lid Switch Handling
bindl = , switch:on:Lid Switch, exec, hyprctl keyword monitor "eDP-1, disable"
bindl = , switch:off:Lid Switch, exec, hyprctl keyword monitor "eDP-1, 1920x1080@60, auto, 1"


# ---------------------
# ---- MY PROGRAMS ----
# ---------------------
$terminal = ghostty
$fileManager = nautilus
$menu = hyprlauncher


# -------------------
# ---- AUTOSTART ----
# -------------------
exec-once = nm-applet
exec-once = /usr/lib/polkit-gnome/polkit-gnome-authentication-agent-1
exec-once = hyprpaper
exec-once = sh -c elephant
exec-once = sh -c 'cd /home/ubuntu/walker && ./target/release/walker'
exec-once = fcitx5 -d



# -------------------------------
# ---- ENVIRONMENT VARIABLES ----
# -------------------------------
env = XCURSOR_SIZE,24
env = HYPRCURSOR_SIZE,24
# Green Tunnel Proxy Configuration
env = http_proxy,http://127.0.0.1:8080/
env = https_proxy,http://127.0.0.1:8080/
env = HTTP_PROXY,http://127.0.0.1:8080/
env = HTTPS_PROXY,http://127.0.0.1:8080/
env = no_proxy,localhost,127.0.0.1,.local
env = NO_PROXY,localhost,127.0.0.1,.local


# -----------------------
# ---- LOOK AND FEEL ----
# -----------------------
general {
    gaps_in = 0
    gaps_out = 0
    border_size = 2
    col.active_border = rgba(33ccffee) rgba(00ff99ee) 45deg
    col.inactive_border = rgba(595959aa)
    resize_on_border = false
    allow_tearing = false
    layout = dwindle
}

decoration {
    active_opacity = 1.0
    inactive_opacity = 1.0

    shadow {
        enabled = true
        range = 4
        render_power = 3
        color = 0xee1a1a1a
    }

    blur {
        enabled = true
        size = 3
        passes = 1
        vibrancy = 0.1696
    }
}

animations {
    enabled = yes
    bezier = easeOutQuint, 0.23, 1, 0.32, 1
    bezier = easeInOutCubic, 0.65, 0.05, 0.36, 1
    bezier = linear, 0, 0, 1, 1
    bezier = almostLinear, 0.5, 0.5, 0.75, 1
    bezier = quick, 0.15, 0, 0.1, 1

    animation = global, 1, 10, default
    animation = border, 1, 5.39, easeOutQuint
    animation = windows, 1, 4.79, default
    animation = windowsIn, 1, 4.1, easeOutQuint, popin 87%
    animation = windowsOut, 1, 1.49, linear, popin 87%
    animation = fadeIn, 1, 1.73, almostLinear
    animation = fadeOut, 1, 1.46, almostLinear
    animation = fade, 1, 3.03, quick
    animation = layers, 1, 3.81, easeOutQuint
    animation = layersIn, 1, 4, easeOutQuint, fade
    animation = layersOut, 1, 1.5, linear, fade
    animation = fadeLayersIn, 1, 1.79, almostLinear
    animation = fadeLayersOut, 1, 1.39, almostLinear
    animation = workspaces, 1, 1.94, almostLinear, fade
    animation = workspacesIn, 1, 1.21, almostLinear, fade
    animation = workspacesOut, 1, 1.94, almostLinear, fade
}

dwindle {
    preserve_split = true
}

master {
    new_status = master
}

misc {
    force_default_wallpaper = -1
    disable_hyprland_logo = false
    font_family = PingFang SC Bold
    splash_font_family = PingFang SC Bold
}

# layers {
#     allow_new_top_layers_over_existing_fullscreen = true
# }



# ---------------
# ---- INPUT ----
# ---------------
input {
    kb_layout = us
    kb_variant = 
    kb_model = 
    kb_options = 
    kb_rules = 

    follow_mouse = 1
    sensitivity = 0.5

    touchpad {
        natural_scroll = true
    }
}

gesture = 3, horizontal, workspace

device {
    name = epic-mouse-v1
    sensitivity = -0.5
}


# ---------------------
# ---- KEYBINDINGS ----
# ---------------------
$mainMod = SUPER

bind = $mainMod, Q, exec, ghostty
bind = $mainMod, C, killactive
bind = $mainMod, M, exec, command -v hyprshutdown >/dev/null 2>&1 && hyprshutdown || hyprctl dispatch exit
bind = $mainMod, E, exec, $fileManager
bind = $mainMod, V, togglefloating
bind = $mainMod, P, pseudo
bind = $mainMod, J, togglesplit

# Move focus with mainMod + arrow keys
bind = $mainMod, left, movefocus, l
bind = $mainMod, right, movefocus, r
bind = $mainMod, up, movefocus, u
bind = $mainMod, down, movefocus, d

# Switch workspaces with mainMod + [0-9]
bind = $mainMod, 1, workspace, 1
bind = $mainMod, 2, workspace, 2
bind = $mainMod, 3, workspace, 3
bind = $mainMod, 4, workspace, 4
bind = $mainMod, 5, workspace, 5
bind = $mainMod, 6, workspace, 6
bind = $mainMod, 7, workspace, 7
bind = $mainMod, 8, workspace, 8
bind = $mainMod, 9, workspace, 9
bind = $mainMod, 0, workspace, 10

# Move active window to a workspace with mainMod + SHIFT + [0-9]
bind = $mainMod SHIFT, 1, movetoworkspace, 1
bind = $mainMod SHIFT, 2, movetoworkspace, 2
bind = $mainMod SHIFT, 3, movetoworkspace, 3
bind = $mainMod SHIFT, 4, movetoworkspace, 4
bind = $mainMod SHIFT, 5, movetoworkspace, 5
bind = $mainMod SHIFT, 6, movetoworkspace, 6
bind = $mainMod SHIFT, 7, movetoworkspace, 7
bind = $mainMod SHIFT, 8, movetoworkspace, 8
bind = $mainMod SHIFT, 9, movetoworkspace, 9
bind = $mainMod SHIFT, 0, movetoworkspace, 10

# Special workspace (scratchpad)
bind = $mainMod, S, togglespecialworkspace, magic
bind = $mainMod SHIFT, S, movetoworkspace, special:magic

# Scroll through existing workspaces with mainMod + scroll
bind = $mainMod, mouse_down, workspace, e+1
bind = $mainMod, mouse_up, workspace, e-1

# Move/resize windows with mainMod + LMB/RMB and dragging
bindm = $mainMod, mouse:272, movewindow
bindm = $mainMod, mouse:273, resizewindow

# Laptop multimedia keys for volume and LCD brightness
bindel = , XF86AudioRaiseVolume, exec, wpctl set-volume -l 1 @DEFAULT_AUDIO_SINK@ 5%+
bindel = , XF86AudioLowerVolume, exec, wpctl set-volume @DEFAULT_AUDIO_SINK@ 5%-
bindl = , XF86AudioMute, exec, wpctl set-mute @DEFAULT_AUDIO_SINK@ toggle
bindl = , XF86AudioMicMute, exec, wpctl set-mute @DEFAULT_AUDIO_SOURCE@ toggle
bindel = , XF86MonBrightnessUp, exec, brightnessctl -e4 -n2 set 5%+
bindel = , XF86MonBrightnessDown, exec, brightnessctl -e4 -n2 set 5%-

# Media control via playerctl
bindl = , XF86AudioNext, exec, playerctl next
bindl = , XF86AudioPause, exec, playerctl play-pause
bindl = , XF86AudioPlay, exec, playerctl play-pause
bindl = , XF86AudioPrev, exec, playerctl previous

# Hyprlock
bind = $mainMod, L, exec, hyprlock

# Screenshots
bind = SUPER SHIFT, S, exec, grim -g "$(slurp)" - | wl-copy

# Walker launcher
bind = $mainMod, R, exec, sh -c 'cd /home/ubuntu/walker && ./target/release/walker'

# Suspend/Sleep laptop
bind = $mainMod, Escape, exec, systemctl suspend && hyprlock


# --------------------------------
# ---- WINDOWS AND WORKSPACES ----
# --------------------------------

windowrule = match:class .*, suppress_event maximize
windowrule = match:class ^$, match:title ^$, match:xwayland true, match:float true, match:fullscreen false, match:pin false, no_focus on
windowrule = match:class ^(hyprland-run)$, float on
windowrule = match:class ^(hyprland-run)$, move 20 120
```
