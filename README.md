# autoptimize-setup-wizard
Added guided Setup Wizard feature

# Autoptimize Setup Wizard - Implementation Guide

This adds a guided, safe setup wizard to Autoptimize to help users configure without breaking sites.

## Features
- Backup reminder first
- Basic site detection
- Step-by-step optimization (JS, CSS, HTML, extras)
- Incremental applies with undo potential
- Final Pro CTA matching autoptimize.com/pro/ style
- Mobile-friendly, day/night mode ready

## Installation
1. Add the new files/folders as described in the code steps.
2. The wizard appears under Autoptimize > Setup Wizard.
3. Activation shows a one-time notice.

## Why This Helps
- Addresses commonality: "(",)"
- Educates users, delivers value fast → higher retent.
- Proven results → conversions.

## Future Ideas
- Add more detection (conflicts, WooCommerce).
- "Test site" button.
- Pro presets.

Integrate into main plugin file
Open autoptimize.php
After the last require_once for classes (around line 40-60, after config/class), add:

require_once __DIR__ . '/classes/autoptimizeWizard.php';


Add activation notice
In autoptimize.php, add near the bottom (after other hooks):

register_activation_hook( __FILE__, function() {
    if ( ! get_option( 'autoptimize_setup_wizard_shown' ) ) {
        set_transient( 'ao_wizard_notice', true, WEEK_IN_SECONDS );
        update_option( 'autoptimize_setup_wizard_shown', true );
    }
} );

add_action( 'admin_notices', function() {
    if ( get_transient( 'ao_wizard_notice' ) && current_user_can( 'manage_options' ) ) {
        ?>
        <div class="notice notice-info is-dismissible">
            <p>Thanks for installing/updating Autoptimize! <strong>Run the new Setup Wizard</strong> for easy optimization: <a href="<?php echo admin_url( 'admin.php?page=ao-setup-wizard' ); ?>">Start Wizard Now</a></p>
        </div>
        <?php
        delete_transient( 'ao_wizard_notice' );
    }
} );
