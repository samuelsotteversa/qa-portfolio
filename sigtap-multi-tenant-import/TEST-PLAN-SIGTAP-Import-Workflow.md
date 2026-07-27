# 📋 Test Plan: SIGTAP Multi-Tenant Import & Procedure Control Engine

## 1. 🎯 Strategy & Scope
This test plan covers the batch import of SIGTAP database updates from the Manager portal to multiple tenant schemas, alongside procedural override protections in local clinics.

### In-Scope
* Manager Portal UI upload flow and modal controls.
* Server configuration limits (Nginx / PHP-FPM upload capacities and directory permissions).
* Worker queue orchestration (sequential processing, worker memory, timeout handling, retry mechanism).
* Invalid archive handling (Corrupted ZIPs, missing required files, duplicate files).
* Local procedure switches (`Revoked` vs. `Update via SIGTAP`).
* Healthcare Regulation & TFD scheduling restriction logic.

### Out-of-Scope
* External SUS API endpoints outside SIGTAP file definitions.

---

## 2. ⚙️ Environment & Middleware Setup
Before executing test cases, verify that the environment permissions and upload limits match high-capacity dataset requirements:

```bash
# Verify Nginx and PHP-FPM upload limits
php -r "echo ini_get('upload_max_filesize').PHP_EOL;" # Expected >= 512M
php -r "echo ini_get('post_max_size').PHP_EOL;"       # Expected >= 520M

# Storage Directory Permissions Validation
sudo chown -R www-data:www-data /var/www/storage
sudo chmod -R 775 /var/www/storage
