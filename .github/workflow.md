# GitHub Workflows Documentation

## Snake Animation Workflow (`snake.yml`)

### Overview
The snake.yml workflow automatically generates an animated snake contribution graph visualization that displays your GitHub activity over time. This animation appears on your profile README and provides a visual representation of your commit history.

### How It Works

#### Triggers
The workflow is triggered in three ways:

1. **Scheduled (Every 12 hours)**
   - Runs automatically every 12 hours via cron schedule: `0 */12 * * *`
   - Ensures the animation stays up-to-date with your latest contributions

2. **Manual Trigger**
   - Can be manually triggered via `workflow_dispatch`
   - Useful for forcing an immediate update without waiting for the schedule

3. **Push Events**
   - Runs when you push to `main` or `master` branch
   - Automatically regenerates the animation after new commits

#### Job Steps

**Step 1: Checkout Repository**
- Action: `actions/checkout@v3`
- Purpose: Downloads the repository code into the workflow environment
- Necessary for accessing repository data and configuration

**Step 2: Generate Snake Animation**
- Action: `Platane/snk/svg-only@v3`
- Purpose: Creates the snake.svg file based on GitHub contribution data
- Configuration:
  - `github_user_name`: Uses the repository owner's username (automatically detected)
  - `outputs`: Generates `dist/snake.svg` with `github-dark` palette theme
  - Uses your contribution history to animate the snake movement

**Step 3: Push to Output Branch**
- Action: `crazy-max/ghaction-github-pages@v3.1.0`
- Purpose: Pushes the generated snake.svg to the `output` branch
- Configuration:
  - `target_branch`: Deploys to the `output` branch (not main)
  - `build_dir`: Uploads the `dist` directory contents
  - `commit_message`: Custom message for tracking updates
  - Uses `GITHUB_TOKEN` for authentication

#### Output Files
- **Location**: `output` branch
- **File**: `snake.svg`
- **Format**: SVG (Scalable Vector Graphics)
- **Theme**: GitHub Dark theme with color palette matching GitHub's dark mode

#### Performance Settings
- **Timeout**: 10 minutes (sufficient for large contribution histories)
- **Permissions**: `contents: write` (required to push to output branch)
- **Environment**: `ubuntu-latest`

### Usage in README

To display the snake animation in your README:

```markdown
<div align="center">
  <img src="https://raw.githubusercontent.com/YOUR_USERNAME/YOUR_USERNAME/output/snake.svg" alt="Snake Animation" />
</div>
```

Replace `YOUR_USERNAME` with your GitHub username.

### Benefits

1. **Visual Engagement**: Eye-catching animation on your profile
2. **Activity Tracking**: Shows contribution patterns over time
3. **Automatic Updates**: No manual intervention needed
4. **Professional Appearance**: Demonstrates active development

### Troubleshooting

If the snake animation isn't updating:

1. Check workflow runs in Actions tab
2. Verify `output` branch was created
3. Ensure correct URL format in README
4. Check that GITHUB_TOKEN has proper permissions
5. Verify workflow file syntax is correct

### Customization

To modify the animation:

1. Change cron schedule for different update frequency
2. Add/remove branch triggers as needed
3. Modify palette parameter for different color schemes
4. Adjust timeout if needed for extensive contribution histories
