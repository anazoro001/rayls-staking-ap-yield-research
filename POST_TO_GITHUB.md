# How to publish this research repository on GitHub

This package is ready to upload as a new repository. Review the evidence and limitations in `README.md` before publishing. Do not present it as an official Rayls audit or imply that the open issue is a verified mainnet vulnerability.

## Option A — GitHub website (no terminal)

1. Download and extract `rayls-staking-ap-yield-research.zip` on your computer.
2. Sign in to GitHub and open https://github.com/new.
3. Choose a repository name, for example `rayls-staking-ap-yield-research`.
4. Add a short description such as: `Independent snapshot comparison and read-only checks of Rayls staking APY settings.`
5. Choose **Public** if you intend to share the research openly, or **Private** if you want to review it before release. Do not select “Add a README” because this package already includes one.
6. Click **Create repository**.
7. In the empty repository, choose **Add file → Upload files**.
8. Open the extracted folder and upload `README.md`, `SOURCES.md`, and the `evidence` contents. If GitHub's web uploader does not preserve the folder structure when dragging, create an `evidence` folder using **Add file → Create new file**, name the file `evidence/README.md`, commit it, and then upload the images into that folder using **Add file → Upload files** while inside the folder.
9. Enter a commit message such as `Add Rayls staking APY research and evidence`.
10. Click **Commit changes**. Open the repository's main page and confirm the README renders correctly and the evidence image links work.

## Option B — Git command line

Extract the ZIP, open a terminal in the extracted `rayls-staking-ap-yield-research` folder, then run:

```bash
git init
git add README.md SOURCES.md evidence
git commit -m "Add Rayls staking APY research and evidence"
git branch -M main
git remote add origin https://github.com/YOUR-USERNAME/rayls-staking-ap-yield-research.git
git push -u origin main
```

Replace `YOUR-USERNAME` with your GitHub username and create the empty repository on GitHub first. Git may ask you to authenticate through your browser or credential manager.

## Suggested repository description

`Independent research into Rayls staking dashboard snapshots, read-only APY getter values, and related public Axyl issues/PRs.`

## Suggested topics

`rayls`, `staking`, `blockchain-research`, `smart-contracts`, `on-chain-analysis`, `defi`

## Before you publish

- Verify the first snapshot's date and figures against the original image if available. The supplied bundle marks the August 31 figures as transcribed from researcher-provided comparison material.
- Keep the selector mapping straight: `0x8265ea06` → `targetApyBps()`; `0x05cdbe8c` → `openTierTargetApyBps()`.
- Do not call 18% a guaranteed yield, effective APY, or contract maximum. The curve addresses were not queried.
- Do not claim Issue #178 proves mainnet behaviour; it reports devnet observations.
- The simulated reads' exact block number was not recorded.
- Add any additional original screenshots to `evidence/` before publishing, and update the evidence index accordingly.

## Optional: publish the six-slide presentation

If you want to share the visual summary too, export it as PDF or place the six slide images in a separate `presentation/` directory. Keep the README as the source of the detailed data and caveats, because some slide screenshots may be too small to independently inspect.
