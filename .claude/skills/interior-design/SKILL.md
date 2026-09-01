---
name: interior-design
description: REQUIRED for all interior design and interior visualization requests. Generate and edit interior design images using Nano Banana (Gemini CLI) — room renders, mood boards, furniture layouts, color palettes, style makeovers, and before/after renovation visuals. Use this skill whenever the user asks to design, decorate, furnish, stage, restyle, or visualize a room, apartment, house interior, or any interior space.
allowed-tools: Bash(gemini:*)
---

# Interior Design (Nano Banana)

Generate professional interior-design visuals via the Gemini CLI's nanobanana extension.
This skill adapts the general-purpose [nano-banana image skill](https://github.com/kkoppenhaver/cc-nano-banana)
specifically for interior design work: room renders, mood boards, furniture layouts,
color palettes, and before/after makeovers.

## When to Use This Skill

ALWAYS use this skill when the user:
- Wants a room, apartment, or house interior designed or redecorated
- Asks for a mood board, color palette, or style concept for a space
- Wants furniture layout ideas or a furnished render of an empty room
- Asks to restyle a room in a specific design style (e.g. Scandinavian, minimalist, boho, industrial, mid-century modern, Japandi, modern Arabic/Majlis style)
- Wants a "before/after" renovation or staging visual
- Uses words like: صمم / design, ديكور / decorate, أثث / furnish, غرفة / room, ستايل / style, mood board, layout, renovate, stage

Do NOT attempt to generate images through any other method.

## Before First Use

1. Verify extension is installed:
   ```bash
   gemini extensions list | grep nanobanana
   ```
2. If missing, install it:
   ```bash
   gemini extensions install https://github.com/gemini-cli-extensions/nanobanana
   ```
3. Verify API key is set:
   ```bash
   [ -n "$GEMINI_API_KEY" ] && echo "API key configured" || echo "Missing GEMINI_API_KEY"
   ```

## Command Selection

| User Request | Command |
|--------------|---------|
| "design my living room in Scandinavian style" | `/generate` |
| "furnish this empty room" | `/generate` or `/edit` (if a photo is provided) |
| "make a mood board for a boho bedroom" | `/generate` |
| "change the sofa/wall color in this photo" | `/edit` |
| "restore this old floor plan photo" | `/restore` |
| "show a before/after of this renovation" | `/edit` (generate the "after" from the "before" photo) |
| "give me a repeating pattern for the wallpaper/tiles" | `/pattern` |
| "draw a floor plan / layout diagram" | `/diagram` |
| "show the design evolving room by room" | `/story` |

## Available Commands

**Note:** Always use the `--yolo` flag to automatically approve all tool actions.

| Command | Use Case |
|---------|----------|
| `gemini --yolo "/generate 'prompt'"` | Full room renders, mood boards, concept art |
| `gemini --yolo "/edit room.png 'instruction'"` | Restyle/furnish an existing room photo |
| `gemini --yolo "/restore old_photo.jpg 'fix scratches'"` | Repair old interior/floor-plan photos |
| `gemini --yolo "/pattern 'description'"` | Wallpaper, rugs, tile, and fabric patterns |
| `gemini --yolo "/diagram 'description'"` | Floor plans, furniture layout diagrams |
| `gemini --yolo "/story 'description'"` | Multi-room or step-by-step design reveal |
| `gemini --yolo "/nanobanana prompt"` | Natural language interface for anything else |

## Common Options

- `--yolo` - **Required.** Auto-approve all tool actions (no confirmation prompts)
- `--count=N` - Generate N variations (1-8) — great for offering style options
- `--preview` - Auto-open generated images
- `--styles="style1,style2"` - Apply design styles (e.g. "scandinavian,minimalist")
- `--format=grid|separate` - Output arrangement (grid is good for mood boards)

## Common Sizes

| Use Case | Dimensions | Notes |
|----------|------------|-------|
| Full room render | 1600x900 | `--aspect=16:9` |
| Mood board | 1080x1080 | Square grid of swatches/pieces |
| Before/after pair | 1200x630 | Easy to share side by side |
| Vertical walkthrough | 1080x1920 | `--aspect=9:16` |

## Model Selection

Default: `gemini-2.5-flash-image` (~$0.04/image)

For higher quality (4K, better spatial reasoning — recommended for realistic room renders):
```bash
export NANOBANANA_MODEL=gemini-3-pro-image-preview
```

## Interior Design Prompt Recipes

Always describe: **room type, style, key furniture/materials, color palette, lighting, camera angle**, and add "no text" / "photorealistic" as needed.

```bash
# Full room render
gemini --yolo "/generate 'photorealistic living room in Scandinavian style, light oak floors, white walls, low-profile beige sofa, wool throw, green plants, warm afternoon light, wide-angle eye-level shot, no text' --preview"

# Furnish an empty room from a photo
gemini --yolo "/edit empty-room.jpg 'furnish this room in minimalist Japandi style: low platform bed, walnut wood accents, linen bedding, paper lantern lamp, keep the room's walls/windows/perspective unchanged'"

# Restyle an existing room to a new style
gemini --yolo "/edit living-room.jpg 'restyle into modern industrial: exposed brick accent wall, black metal shelving, leather sofa, Edison bulb pendant lights, keep layout and windows the same'"

# Mood board
gemini --yolo "/generate 'flat-lay interior design mood board for a boho bedroom: rattan headboard swatch, terracotta and cream color palette, macrame wall hanging, dried pampas grass, warm wood tones, grid layout, no text' --format=grid"

# Color palette exploration
gemini --yolo "/generate 'three color palette swatches for a modern kitchen: sage green cabinets, warm palette; navy blue cabinets, cool palette; terracotta cabinets, earthy palette' --count=3"

# Before/after renovation
gemini --yolo "/edit outdated-kitchen.jpg 'renovate into a modern kitchen: white shaker cabinets, quartz countertops, matte black hardware, subway tile backsplash, keep the same layout and window position'"

# Floor plan / furniture layout diagram
gemini --yolo "/diagram '3x4 meter bedroom furniture layout with a queen bed, wardrobe, and desk, top-down floor plan style, labeled dimensions'"

# Wallpaper / tile pattern
gemini --yolo "/pattern 'seamless botanical wallpaper pattern, sage green leaves on cream background, hand-drawn style'"
```

## Output Location

All generated images are saved to `./nanobanana-output/` in the current directory.

## Presenting Results

After generation completes:
1. List contents of `./nanobanana-output/` to find generated files
2. Present the most recent image(s) to the user
3. Offer to regenerate with variations (different style, palette, or layout) if needed

## Refinements and Iterations

When the user asks for changes:
- **"Give me other styles"**: Regenerate with `--count=3`, varying `--styles=`
- **"Warmer/cooler colors"**: Adjust the color palette in the prompt and regenerate
- **"Change just the sofa/wall/flooring"**: Use `gemini --yolo "/edit nanobanana-output/filename.png 'adjustment'"`
- **"Show the before and after together"**: Generate both and mention they can be placed side by side

## Prompt Tips

1. **Name the room type and dimensions** if known (e.g. "12x14 ft bedroom")
2. **Name the style explicitly**: Scandinavian, minimalist, bohemian, industrial, mid-century modern, Japandi, modern Arabic/Majlis, coastal, farmhouse
3. **Specify materials and palette**: wood tones, metal finishes, fabric types, 2-3 dominant colors
4. **Specify lighting and camera angle**: "warm afternoon light", "eye-level wide shot", "top-down floor plan"
5. **When editing a real photo, tell it what to preserve**: "keep the walls, windows, and perspective unchanged"
6. **Add "no text"** unless labels (e.g. floor plan dimensions) are wanted

## Troubleshooting

| Problem | Solution |
|---------|----------|
| `GEMINI_API_KEY` not set | `export GEMINI_API_KEY="your-key"` |
| Extension not found | Run install command from setup section |
| Quota exceeded | Wait for reset or switch to flash model |
| Image generation failed | Check prompt for policy violations, simplify request |
| Output directory missing | Will be created automatically on first run |
| Room geometry changes unexpectedly on `/edit` | Explicitly instruct to preserve walls/windows/perspective in the prompt |
