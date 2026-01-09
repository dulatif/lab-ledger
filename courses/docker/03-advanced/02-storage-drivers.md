# 2. Storage Drivers (OverlayFS)

## 🎯 Learning Goal

How the Union Filesystem actually works deep down.

## 🧠 Concept

Docker supports multiple storage drivers (`overlay2`, `btrfs`, `zfs`, `vfs`).
**Overlay2** is the current standard.

### How Overlay Work

It takes multiple directories (layers) and stack them creates a single merged view.

- **LowerDir**: The Read-only image layers.
- **UpperDir**: The Read-write container layer.
- **MergedDir**: The unified view that the application sees.
- **WorkDir**: Internal usage for atomicity.

### Mechanics

1.  **Read**: If file exists in Upper, read Upper. If not, look in Lower.
2.  **Delete**: If deleting a Lower file, a "Whiteout" file (marker) is created in Upper to hide the Lower file.
3.  **Modify**: File copied from Lower to Upper (up) and modified there.

## 💻 Deep Dive: Inodes

The filesystem uses "Hard Links" aggressively to save space between images that share layers.

## 🧩 Activity / Challenge

1.  This explains why `docker build` is fast but `docker export/save` can be slow (needs to flatten the layers).

## 🔑 Key Takeaways

- Excessive Layering can impact I/O performance (kernel has to look through deep stack).
- Keep filesystems flat where possible.
