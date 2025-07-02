## Setup Context 7 MCP
claude mcp add context7 -s project -- npx -y @upstash/context7-mcp@latest


# Enable Bedrock integration Each time
export CLAUDE_CODE_USE_BEDROCK=1
export AWS_REGION=us-east-1  # or your preferred region

# Enable Bedrock intgration as part of your shell (macOS zsh && ohmyzsh)
echo -e '\n# Enable Bedrock integration\nexport CLAUDE_CODE_USE_BEDROCK=1\nexport AWS_REGION=us-east-1' >> ~/.zshrc && source ~/.zshrc