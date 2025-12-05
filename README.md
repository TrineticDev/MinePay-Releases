# MinePay - Minecraft PayPal Integration Plugin

A comprehensive Minecraft plugin that enables PayPal payments directly in-game with a beautiful GUI shop system, custom variables, shopping cart, and Discord webhook notifications.

---

## Table of Contents

- [Features](#features)
- [Installation](#installation)
- [Quick Start](#quick-start)
- [Commands](#commands)
- [Configuration](#configuration)
  - [config.yml](#configyml)
  - [shop.yml](#shopyml)
  - [variables.yml](#variablesyml)
  - [Locales.yml](#localesyml)
- [Custom Variables System](#custom-variables-system)
- [Discord Webhooks](#discord-webhooks)
- [Requirements System](#requirements-system)
- [Shopping Cart](#shopping-cart)
- [Pending Rewards](#pending-rewards)
- [Permissions](#permissions)
- [Placeholders](#placeholders)
- [FAQ](#faq)

---

## Features

✨ **Core Features:**
- 🛒 **In-Game Shop GUI** - Beautiful, customizable shop interface
- 💳 **PayPal Integration** - Secure PayPal payment processing
- 🛍️ **Shopping Cart** - Add multiple items before checkout
- 🎁 **Gift System** - Purchase items as gifts for other players
- 🎨 **Custom Variables** - Tebex-style variable system for customizable purchases
- 📦 **Pending Rewards** - Offline players receive purchases when they log in
- 🔔 **Discord Notifications** - Professional admin webhook notifications
- 💰 **Creator Codes** - Support content creators with referral codes
- 🔒 **Requirements System** - Restrict purchases based on placeholders
- 🌐 **Multi-Currency Support** - USD, EUR, GBP, JPY, and more
- 📊 **Order Limit** - Maximum transaction limit of $1,000 per order

---

## Installation

1. **Download MinePay** and place the JAR file in your server's `plugins/` folder
2. **Restart your server** to generate the default configuration files
3. **Configure** the plugin (see [Configuration](#configuration))
4. **Link your server** using `/minepay link <token>` (get token from MinePay dashboard)
5. **Open the shop** using `/shop`, `/buy`, or `/store`

### Requirements
- Minecraft Server (Spigot, Paper, or compatible fork)
- Java 8 or higher
- Internet connection for PayPal API calls

### Optional Dependencies
- **PlaceholderAPI** - For advanced placeholder support in requirements

---

## Quick Start

### Basic Setup

1. **Edit `shop.yml`** to add your shop categories and items:
```yaml
categories:
  Ranks:
    title: "&6&lServer Ranks"
    display: "DIAMOND"
    slot: 10
    items:
      - name: "&aVIP Rank"
        material: "EMERALD"
        price: 5.00
        slot: 11
        commands:
          - "lp user %player% parent set vip"
```

2. **Configure Discord webhooks** in `config.yml` (optional but recommended)

3. **Test your shop** with `/shop`

4. **Link your server** with `/minepay link <token>`

---

## Commands

| Command | Description | Permission | Aliases |
|---------|-------------|------------|---------|
| `/shop` | Open the shop GUI | `minepay.shop` | `/buy`, `/store` |
| `/minepay` | Main plugin command | `minepay.admin` | - |
| `/minepay help` | Show help message | `minepay.admin` | - |
| `/minepay setup` | Initial setup guide | `minepay.admin` | - |
| `/minepay reload` | Reload all configurations | `minepay.admin` | - |
| `/minepay admin` | Open admin management GUI | `minepay.admin` | - |
| `/minepay link <token>` | Link server with auth token | `minepay.admin` | - |
| `/minepay unlink` | Remove stored server token | `minepay.admin` | - |

---

## Configuration

### config.yml

The main configuration file for MinePay.

```yaml
# Debug settings
debug:
  enabled: true  # Enable debug logging

# Webhook server configuration
webhook:
  port: 8080  # Port for payment callbacks (tries alternatives if unavailable)

# Discord webhook notifications
discord:
  enabled: false  # Enable Discord notifications
  url: "https://discord.com/api/webhooks/YOUR_WEBHOOK_URL"
  
  settings:
    notify_success: true  # Send notification on successful payment
    notify_failed: true   # Send notification on failed payment
  
  # Success message customization
  success_message:
    username: "MinePay Transaction System"
    avatar_url: ""
    embed:
      title: "✅ Transaction Completed"
      description: "A successful payment transaction has been processed and items have been delivered to the player."
      color: 3066993  # Green color in decimal (use https://www.spycolor.com/ to convert hex)
      # Available placeholders: {player}, {amount}, {currency}, {items}, {transaction_id}, {order_id}, {timestamp}
  
  # Failed message customization
  failed_message:
    username: "MinePay Transaction System"
    embed:
      title: "⚠️ Transaction Failed - Action Required"
      description: "A payment transaction has failed and requires administrative attention."
      color: 15105570  # Orange color in decimal

# Command configuration
commands:
  store:
    name: "buy"           # Main command
    aliases: ["shop", "store"]  # Alternative commands

# GUI configuration
gui:
  size:
    type: "double"  # 'single' (27 slots) or 'double' (54 slots)
  spacing:
    enabled: true   # Add spacing between items
    amount: 1       # Number of empty slots between items
```

#### Discord Webhook Configuration

**Available Placeholders:**
- `{player}` - Player username
- `{amount}` - Payment amount
- `{currency}` - Currency code (e.g., USD)
- `{items}` - List of purchased items
- `{transaction_id}` - PayPal transaction ID
- `{order_id}` - PayPal order ID
- `{timestamp}` - Transaction timestamp
- `{reason}` - Failure reason (failed messages only)

**Color Codes:**
Use [SpyColor](https://www.spycolor.com/) to convert hex colors to decimal:
- Green (#2ECC71): `3066993`
- Orange (#E67E22): `15105570`
- Red (#E74C3C): `15158332`
- Blue (#3498DB): `3447003`

---

### shop.yml

Define your shop categories, items, and variables.

#### Basic Structure

```yaml
currency: usd  # Supported: usd, eur, gbp, jpy, cny, cad, aud, inr, krw, brl, mxn, chf, sek, nok, dkk, rub

categories:
  CategoryName:
    title: "&e&lCategory Title"
    display: "MATERIAL_NAME"
    slot: 10
    items:
      - name: "&aItem Name"
        material: "MATERIAL_NAME"
        price: 10.00
        slot: 11
        lore:
          - "&7Item description line 1"
          - "&7Item description line 2"
        commands:
          - "command to execute %player%"
```

#### Item Properties

| Property | Type | Required | Description |
|----------|------|----------|-------------|
| `name` | String | Yes | Display name (supports color codes) |
| `material` | String | Yes | Minecraft material type |
| `price` | Number | Yes | Base price in configured currency |
| `slot` | Number | Yes | GUI slot position (0-53) |
| `lore` | List | No | Item description lines |
| `commands` | List | No | Commands to execute on purchase |
| `submenu` | Object | No | Quantity selection submenu |
| `requirements` | List | No | Purchase requirements |
| `variables` | List/Array | No | Custom variables (see below) |

#### Quantity Submenu

Allow players to select quantities with bulk pricing:

```yaml
submenu:
  type: quantity
  amounts: [1, 8, 16, 32, 64]
  prices:
    1: 10.00
    8: 75.00    # Bulk discount
    16: 140.00
    32: 260.00
    64: 480.00
```

#### Requirements System

Restrict purchases based on placeholders:

```yaml
requirements:
  - placeholder: "luckperms_primary_group"
    equals: "vip"
    fail_message: "&cYou must be VIP to purchase this!"
  
  - placeholder: "%vault_eco_balance%"
    greater_than: 1000
    fail_message: "&cYou need at least $1,000 in-game money!"
```

**Supported Operators:**
- `equals` - Exact match
- `not_equals` - Not equal to
- `contains` - Contains substring
- `greater_than` - Number comparison (>)
- `less_than` - Number comparison (<)

---

### variables.yml

Define reusable custom variables for shop items.

#### Basic Structure

```yaml
variables:
  variable_key:
    name: "variable_name"           # Used in {variable:name} placeholder
    display_name: "&eVariable Title"
    required: true
    description:
      - "&7Help text line 1"
      - "&7Help text line 2"
    gui:
      title: "&6Choose Option"
      size: 27                      # 9, 18, 27, 36, 45, or 54
      fill_empty: true              # Fill empty slots
      fill_material: "GRAY_STAINED_GLASS_PANE"
    options:
      - display_name: "&aOption 1"
        material: "DIAMOND"
        slot: 10
        price: 5.00                 # Additional price (added to base item price)
        value: "option_value_1"     # Value inserted into commands
        lore:
          - "&7Description"
          - "&ePrice: +{price}"
```

#### Example: Pokemon Species

```yaml
variables:
  pokemon_species:
    name: "species"
    display_name: "&ePokemon Species"
    required: true
    gui:
      title: "&6Choose Pokemon Species"
      size: 54
    options:
      - display_name: "&cCharizard"
        material: "BLAZE_POWDER"
        slot: 10
        price: 20.00
        value: "Charizard"
      - display_name: "&bBlastoise"
        material: "WATER_BUCKET"
        slot: 11
        price: 15.00
        value: "Blastoise"
```

**Using in shop.yml:**
```yaml
items:
  - name: "&6Custom Pokemon"
    material: "MONSTER_EGG"
    price: 0  # Base price (variable prices add to this)
    variables: [pokemon_species, pokemon_level]
    commands:
      - "pokegive %player% {variable:species} lvl:{variable:level}"
```

---

### Locales.yml

Customize all plugin messages and GUI text.

#### Key Sections

**GUI Titles:**
```yaml
shop_categories_title: "&e&lShop Categories"
select_amount_title: "&aSelect Amount"
confirmation_title: "&6&lConfirm Purchase"
```

**Messages:**
```yaml
payment_success_title: "&a✓ Payment successful! Transaction ID: {transaction_id}"
purchase_cancelled: "&7Purchase cancelled."
pending_rewards_delivered_message: "&aYou received &f{count}&a purchase{plural}!"
```

**Available Placeholders:**
- `{price}` - Item price
- `{amount}` - Quantity
- `{item}` - Item name
- `{player}` - Player name
- `{count}` - Count
- `{transaction_id}` - Transaction ID
- `{category}` - Category name

---

## Custom Variables System

MinePay's variable system allows players to customize their purchases, similar to Tebex.

### How It Works

1. Player clicks an item with variables
2. Opens "Configure Item" GUI
3. Select options for each variable
4. Each option can add to the price
5. Final price = base price + all variable option prices
6. Selected values replace `{variable:name}` in commands

### Three Methods of Definition

#### Method 1: Reference from variables.yml (Recommended)

**In shop.yml:**
```yaml
- name: "&6Custom Pokemon"
  price: 0
  variables: [pokemon_species, pokemon_level, pokemon_nature]
  commands:
    - "pokegive %player% {variable:species} lvl:{variable:level} nature:{variable:nature}"
```

**Benefits:**
- Clean and maintainable
- Define once, reuse everywhere
- Update options in one place

#### Method 2: Inline Definition

**In shop.yml:**
```yaml
- name: "&cCustom Weapon"
  price: 0
  variables:
    - name: "weapon_type"
      display_name: "&cWeapon Type"
      required: true
      gui:
        title: "&cChoose Weapon Type"
        size: 9
      options:
        - display_name: "&bDiamond"
          material: "DIAMOND_SWORD"
          slot: 0
          price: 10.00
          value: "diamond"
  commands:
    - "give %player% {variable:weapon_type}_sword 1"
```

**Use Case:** One-off variables specific to a single item

#### Method 3: Mixed (Reference + Inline)

```yaml
variables:
  - ref: enchant_level           # From variables.yml
  - name: "weapon_type"          # Inline definition
    display_name: "&cWeapon Type"
    ...
```

### Variable Placeholders

Use `{variable:name}` where `name` matches the variable's `name` field:

```yaml
variables:
  pokemon_species:
    name: "species"  # Use {variable:species}
  
  pokemon_level:
    name: "level"    # Use {variable:level}
```

**Command Example:**
```yaml
commands:
  - "pokegive %player% {variable:species} lvl:{variable:level}"
```

If player selects:
- Species: Charizard
- Level: 50

Result: `pokegive Steve Charizard lvl:50`

---

## Discord Webhooks

Professional admin notifications for all transactions.

### Setup

1. Create a webhook in Discord:
   - Server Settings → Integrations → Webhooks → New Webhook
2. Copy the webhook URL
3. Paste into `config.yml` under `discord.url`
4. Set `discord.enabled: true`

### Success Notification Format

```yaml
success_message:
  username: "MinePay Transaction System"
  embed:
    title: "✅ Transaction Completed"
    description: "A successful payment transaction has been processed and items have been delivered to the player."
    color: 3066993
    fields:
      - name: "Transaction Information"
        value: "```yaml\nTransaction ID: {transaction_id}\nOrder ID: {order_id}\nTimestamp: {timestamp}```"
      - name: "Customer Details"
        value: "```yaml\nPlayer: {player}```"
      - name: "Payment Details"
        value: "```yaml\nAmount: ${amount} {currency}```"
```

### Failed Notification Format

```yaml
failed_message:
  username: "MinePay Transaction System"
  embed:
    title: "⚠️ Transaction Failed - Action Required"
    description: "A payment transaction has failed and requires administrative attention."
    color: 15105570
    fields:
      - name: "Error Information"
        value: "```fix\n{reason}```"
```

### Customization Options

**Embed Fields:**
- `name` - Field title
- `value` - Field content (supports markdown)
- `inline` - Display side-by-side (true/false)

**Formatting Tips:**
- Use ` ```yaml ` for structured data
- Use ` ```diff\n- FAILED ` for error highlighting
- Use ` ```fix ` for warning text
- Add separator lines: `━━━━━━━━━━━━━━━━━━━━━`

---

## Requirements System

Restrict item purchases based on conditions.

### Placeholder Support

Requires **PlaceholderAPI** for most placeholders.

**Common Placeholders:**
- `%luckperms_primary_group%` - Player's rank
- `%vault_eco_balance%` - Player's money
- `%player_level%` - Player's level
- `%player_world%` - Current world

### Requirement Operators

```yaml
requirements:
  # String comparison
  - placeholder: "luckperms_primary_group"
    equals: "vip"
    fail_message: "&cVIP only!"
  
  - placeholder: "player_world"
    not_equals: "spawn"
    fail_message: "&cCannot purchase in spawn!"
  
  - placeholder: "luckperms_primary_group"
    contains: "donor"
    fail_message: "&cDonor rank required!"
  
  # Number comparison
  - placeholder: "vault_eco_balance"
    greater_than: 1000
    fail_message: "&cNeed $1,000 in-game!"
  
  - placeholder: "player_level"
    less_than: 50
    fail_message: "&cMust be under level 50!"
```

### Multiple Requirements

All requirements must be met (AND logic):

```yaml
requirements:
  - placeholder: "luckperms_primary_group"
    equals: "vip"
    fail_message: "&cVIP rank required!"
  - placeholder: "vault_eco_balance"
    greater_than: 5000
    fail_message: "&cNeed $5,000 in-game!"
  - placeholder: "player_world"
    equals: "survival"
    fail_message: "&cMust be in survival world!"
```

Player must satisfy ALL three conditions.

---

## Shopping Cart

Add multiple items before checkout.

### Features

- ✅ Add items from different categories
- ✅ Customize each item with variables
- ✅ Single PayPal transaction for entire cart
- ✅ Automatic bulk discount calculation
- ✅ Maximum cart value: $1,000

### How to Use

1. Click an item → Select quantity/options
2. In confirmation GUI, click **"Add More Items"**
3. Item added to cart, returns to shop
4. Select more items
5. Click **"Confirm Purchase"** to checkout

### Cart Processing

- All items processed in ONE PayPal order
- Commands executed for all items
- If player offline, all items stored as pending rewards

---

## Pending Rewards

Offline players automatically receive purchases when they log in.

### How It Works

1. Player purchases while offline
2. Commands stored in `data/orders/pending_orders.json`
3. On login, checks for pending rewards
4. Executes all commands automatically
5. Shows notification to player

### Inventory Space Check

- Requires **5+ empty slots** for delivery
- If insufficient space, waits until inventory cleared
- Shows warning message to player

### Storage Location

```
plugins/MinePay/data/orders/
├── pending_orders.json       # Current pending rewards
├── completed_orders.json     # Order history
└── <uuid>.json              # Individual order files
```

### Messages

```yaml
# Locales.yml
pending_rewards_delivered_message: "&aYou received &f{count}&a purchase{plural}!"
pending_rewards_no_space: "&cPlease free up inventory space (need at least 5 empty slots)"
```

---

## Permissions

| Permission | Description | Default |
|------------|-------------|---------|
| `minepay.shop` | Access shop GUI | All players |
| `minepay.admin` | Admin commands and GUI | Operators |
| `minepay.bypass.requirements` | Bypass purchase requirements | Operators |
| `minepay.reload` | Reload configurations | Operators |

### Permission Setup Examples

**LuckPerms:**
```
/lp group default permission set minepay.shop true
/lp group admin permission set minepay.admin true
```

**PermissionsEx:**
```
/pex group default add minepay.shop
/pex group admin add minepay.admin
```

---

## Placeholders

### Internal Placeholders

Used in commands and locales:

| Placeholder | Description | Example |
|-------------|-------------|---------|
| `%player%` or `{player}` | Player username | `Steve` |
| `{price}` | Item price | `$10.00` |
| `{amount}` | Quantity | `16` |
| `{item}` | Item name | `Diamond Sword` |
| `{transaction_id}` | PayPal transaction ID | `ABC123` |
| `{category}` | Category name | `Weapons` |
| `{variable:name}` | Variable value | `Charizard` |

### PlaceholderAPI Support

MinePay supports PlaceholderAPI in:
- Item names and lore
- Command execution
- Requirements system
- Locale messages

**Example:**
```yaml
lore:
  - "&7Your balance: &e%vault_eco_balance%"
  - "&7Your rank: &b%luckperms_primary_group%"
```

---

## FAQ

### General Questions

**Q: What payment methods are supported?**  
A: Currently only PayPal. More payment processors coming soon.

**Q: What's the maximum transaction amount?**  
A: $1,000 USD per order for security.

**Q: Can I refund purchases?**  
A: Refunds must be processed through PayPal. Commands are executed immediately and cannot be automatically reversed.

**Q: Does this work with offline players?**  
A: Yes! Purchases are stored and delivered when the player logs in.

### Configuration

**Q: How do I change the currency?**  
A: Edit `shop.yml` and change `currency: usd` to your preferred currency code.

**Q: Can I use MiniMessage formatting?**  
A: Not yet. Currently only `&` color codes are supported.

**Q: How do I add more shop pages?**  
A: The GUI automatically paginates when you have more items than fit in one page.

### Variables

**Q: Can variables have variables?**  
A: No, nested variables are not supported.

**Q: Do variable prices stack?**  
A: Yes! Final price = base price + sum of all selected variable option prices.

**Q: Can I make variables optional?**  
A: Set `required: false` in the variable definition.

### Discord Webhooks

**Q: Webhooks not sending?**  
A: Check:
1. `discord.enabled: true` in config.yml
2. Valid webhook URL
3. Discord server hasn't deleted the webhook
4. Server has internet connection

**Q: Can I customize the embed color?**  
A: Yes! Use [SpyColor](https://www.spycolor.com/) to convert hex to decimal.

**Q: Can I add images to webhooks?**  
A: Yes! Set `thumbnail:` or `image:` to an image URL.

### Troubleshooting

**Q: Shop GUI not opening?**  
A: Check:
1. Player has `minepay.shop` permission
2. `shop.yml` has valid syntax
3. Console for error messages

**Q: Commands not executing?**  
A: Check:
1. Command syntax is correct
2. Placeholders are properly formatted
3. Console for command execution logs (enable debug mode)

**Q: PayPal payment not processing?**  
A: Check:
1. Server is linked with `/minepay link <token>`
2. Webhook port is accessible
3. Internet connection is stable
4. API credentials are valid

**Q: Variable selections not saving?**  
A: Variable selections are temporary and only last until purchase completion. This is intentional.

---

## Support

- **Issues:** Report bugs on GitHub Issues
- **Documentation:** Check this README and inline config comments
- **Discord:** Join our support server (link in plugin description)

---

## License

MinePay is proprietary software. All rights reserved.

---

## Credits

Developed by **thawk** with ❤️ for the Minecraft community.

Special thanks to:
- The Paper team for excellent server software
- The Spigot community for continuous support
- All beta testers and early adopters

---

**Made with ☕ and late nights 🌙**
