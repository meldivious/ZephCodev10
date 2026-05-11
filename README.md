# Zephora Logistic Systems

A comprehensive WordPress plugin for logistics services, providing secure frontend logistics platform with KYC-gated SHIP FOR ME & BUY FOR ME workflows.

## Features

- **Ship For Me**: Send packages from US warehouses to Nigeria
- **Buy For Me**: Purchase items in the US and ship to Nigeria
- **KYC Verification**: Secure customer verification system with admin approval workflow
- **User Dashboard**: iOS-inspired frontend UI for tracking requests and managing shipments
- **Admin Dashboard**: Complete management interface with bulk operations
- **Email Notifications**: Automated status update notifications to customers
- **Audit Logging**: Complete activity tracking for compliance
- **GDPR Compliance**: Data protection and privacy features
- **US Warehouse Management**: Configure multiple warehouse addresses for customers

## Requirements

- WordPress 6.4+
- PHP 7.4+
- MySQL 5.6+

## Installation

### Manual Installation

1. Upload the plugin folder to `wp-content/plugins/zephora-logistics/`
2. Activate the plugin through WordPress admin
3. Configure settings in **Zephora Logistics > Settings**

## Activation & Setup

When the plugin is activated, it will automatically:

1. **Create necessary directories**:
   - `wp-content/uploads/zls-kyc-documents/` - KYC document uploads
   - `wp-content/zls-logs/` - Error and audit logs

2. **Create essential pages**:
   - **My Dashboard** (`/my-dashboard/`) - User dashboard with [zls_dashboard] shortcode
   - **KYC Verification** (`/kyc-verification/`) - KYC verification page
   - **Ship For Me** (`/ship-for-me/`) - Shipping request form with [zls_ship_for_me] shortcode
   - **Buy For Me** (`/buy-for-me/`) - Purchase request form with [zls_buy_for_me] shortcode
   - **Login** (`/login/`) - Custom login page (redirects to WordPress login)

3. **Set default configuration options**
4. **Create a default US warehouse address** (Texas Warehouse)
5. **Register custom post types** (zls_ship, zls_buy)
6. **Log the activation event**

### Important Notes:
- Pages are created only if they don't already exist
- Existing pages with the same slugs will not be overwritten
- You can customize page content and settings after activation
- All pages are created as published with comments disabled

## Configuration

### Basic Setup

1. Go to **WordPress Admin > Zephora Logistics > Settings**
2. Configure general settings:
   - Set default tax rates
   - Configure email notification templates
   - Manage US warehouse addresses

### US Warehouse Addresses

1. Go to **Settings > US Addresses**
2. Add multiple warehouse locations
3. Mark addresses as active/inactive
4. Include contact information for each location

Customers will see these addresses when submitting Ship For Me requests.

## Usage

### For Customers

#### Registration & KYC
1. User registers on your site
2. Completes KYC verification form with NIN, phone, and address
3. Admin reviews and approves KYC from WordPress admin
4. User gains access to logistics services (Ship For Me & Buy For Me)

#### Ship For Me Workflow
1. User logs into dashboard at `/my-dashboard/`
2. Navigates to Ship For Me form at `/ship-for-me/`
3. Fills out shipping form with:
   - Product name and description
   - Package details (weight, dimensions)
   - Selected US warehouse address
   - Recipient information in Nigeria
4. Submits request
5. Admin reviews and sends quote
6. Customer receives email notification
7. Customer confirms payment via bank transfer
8. Admin marks as paid and processes shipment

#### Buy For Me Workflow
1. User logs into dashboard
2. Navigates to Buy For Me form at `/buy-for-me/`
3. Provides:
   - Product details and specifications
   - Product URL (if available)
   - Budget in USD
   - Quantity needed
4. Submits request
5. Admin reviews, calculates costs, and sends quote
6. Customer receives email notification
7. Customer confirms payment
8. Zephora team purchases item and ships to Nigeria

### For Administrators

#### Managing Requests
- View all ship/buy requests in **Zephora Logistics > Ship Requests** or **Buy Requests**
- Update request statuses (pending → quote_sent → payment_pending → paid → processing → shipped → delivered)
- Generate and send quotes via email
- Confirm manual bank transfer payments
- Track shipment progress
- Add internal notes to requests

#### KYC Management
- Review pending KYC applications in **Users** section
- Approve or deny applications
- View submitted KYC data (NIN, phone, address)
- Manage user access to logistics services

#### Bulk Operations
Admins can perform bulk actions on multiple requests:
- **Approve & Send Quote** - Send quotes to multiple customers
- **Mark as Paid** - Bulk update status to paid
- **Mark as Shipped** - Bulk update status to shipped
- **Mark as Delivered** - Bulk update status to delivered
- **Send Notification** - Send status notifications to multiple customers

**How to Use:**
1. Go to **Zephora Logistics > Ship Requests** or **Buy Requests**
2. Select multiple requests using checkboxes
3. Choose action from the "Bulk Actions" dropdown
4. Click "Apply"

#### Email Notifications
The plugin sends automated emails for:
- Quote sent to customer
- Payment received confirmation
- Status updates (shipped, delivered, etc.)

Email templates can be customized in **Settings > Email Templates**.

## Shortcodes

The plugin provides the following shortcodes for displaying forms and content:

- `[zls_dashboard]` - User dashboard (shows on My Dashboard page)
- `[zls_ship_for_me]` - Ship For Me request form (shows on Ship For Me page)
- `[zls_buy_for_me]` - Buy For Me request form (shows on Buy For Me page)

### Usage Examples:

```php
// Add to any page or post
[zls_dashboard]
[zls_ship_for_me]
[zls_buy_for_me]
```

## Request Statuses

The plugin uses the following status workflow:

1. **pending** - Initial submission
2. **quote_sent** - Admin has sent a quote
3. **payment_pending** - Awaiting customer payment
4. **paid** - Payment confirmed
5. **purchasing** - Item being purchased (Buy For Me only)
6. **received_us** - Package received at US warehouse
7. **shipped** - Package shipped to Nigeria
8. **delivered** - Package delivered to customer
9. **cancelled** - Request cancelled

## API Endpoints

### AJAX Endpoints
- `wp_ajax_fetch_shipment_details` - Get shipment information (logged-in users)
- `wp_ajax_zls_delete_request` - Delete pending requests
- `wp_ajax_load_recent_activity` - Load paginated activity feed
- `wp_ajax_confirm_payment` - Confirm manual payments from dashboard

## File Structure

```
zephora-logistics/
├── zephora-logistics.php          # Main plugin file
├── README.md                      # This file
├── assets/
│   ├── css/
│   │   ├── admin.css             # Admin styles
│   │   ├── frontend.css          # Frontend styles
│   │   └── kyc-frontend.css      # KYC form styles
│   └── js/
│       ├── admin.js              # Admin scripts
│       ├── frontend.js           # Frontend scripts
│       └── settings.js           # Settings scripts
├── includes/
│   ├── class-core.php            # Core functionality
│   ├── class-dashboard.php       # User dashboard
│   ├── class-ship-for-me.php     # Ship For Me handler
│   ├── class-buy-for-me.php      # Buy For Me handler
│   ├── class-kyc-manager.php     # KYC management
│   ├── class-notifications.php   # Email notifications
│   ├── class-bulk-operations.php # Bulk admin actions
│   ├── class-audit-logger.php    # Audit logging
│   ├── class-admin-ui.php        # Admin interface
│   ├── class-settings.php        # Settings management
│   ├── class-security.php        # Security features
│   ├── class-gdpr.php            # GDPR compliance
│   ├── class-address-manager.php # US warehouse addresses
│   ├── class-request-manager.php # Request handling base
│   ├── class-request-base.php    # Shared request functions
│   ├── class-notes.php           # Internal notes on requests
│   ├── class-error-handler.php   # Error handling & logging
│   ├── class-module-loader.php   # Module loading system
│   └── class-post-types.php      # Custom post types
└── languages/
    └── zls-en_US.mo              # Translation files
```

## Error Handling & Logging

The plugin includes comprehensive error handling and logging:

- **Error Handler**: Catches PHP errors, exceptions, and fatal errors
- **Application Logging**: Logs application-specific errors and events
- **Audit Logging**: Tracks all user actions and system events
- **Log Files**: Located in `wp-content/zls-logs/`
- **Admin Access**: Error logs can be viewed in admin for debugging

## Security

- All forms include nonce verification
- Input sanitization and validation
- User capability checks
- Secure file uploads for KYC documents
- Audit logging for all actions
- GDPR compliance features
- Comprehensive error handling prevents information leakage
- KYC-gated access ensures only verified users can submit requests

## Developer Guide

### Bulk Operations Functions

```php
// Get bulk actions summary for multiple requests
$post_ids = array(123, 124, 125);
$summary = ZLS_Bulk_Operations::get_bulk_actions_summary($post_ids);
// Returns: total_requests, total_value_usd, status_counts, type_counts
```

### Hooks & Filters

The plugin uses standard WordPress hooks for extensibility. Developers can hook into:
- Request status changes
- Email notification sending
- KYC approval/denial events
- Audit log entries

## Changelog

### Version 1.0.18 (Latest)
**Improvements:**
- Removed payment gateway dependencies (Paystack, Flutterwave)
- Removed PDF invoice generation
- Removed export functionality
- Simplified to focus on core logistics workflows
- Streamlined codebase for better maintainability

### Version 1.0.17
- Initial release
- Ship For Me and Buy For Me workflows
- KYC verification system
- Admin management interface
- Email notifications
- Bulk operations support

## Support

For support, please contact:
- Email: support@zephora.logistics
- Website: https://zephora.logistics

## License

GPL-3.0-only - See LICENSE file for details

## Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Add tests if applicable
5. Submit a pull request

## Credits

Developed by Zephora Engineering