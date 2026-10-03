# Contacts

PocketPay lets users keep an address book of Stellar public keys.

## How contacts are stored

- Each contact has an `id`, a `name` and a `publicKey` (a Stellar `G...` address).
- Contacts are saved on the device with AsyncStorage under the key
  `@pocketpay_contacts`, as a JSON array.
- They are loaded when the app starts, and the full list is saved again
  every time a contact is added or removed.
- AsyncStorage is not encrypted. This is acceptable because contacts only
  hold public information (see [storage.md](./storage.md)).
- Contacts can be added manually or by scanning a QR code. Duplicate
  addresses are rejected.
- Contacts are local-only: the app code only reads and writes them in
  AsyncStorage, and no code that sends contacts to a server was found.

## Backup limitations

- There is currently no export, import, backup or sync feature for contacts.
- The app never clears the contact list by itself. Contacts stay in storage
  until the user deletes them one by one from the Contacts screen.
- The app does not define any backup rules for AsyncStorage (there are no
  backup settings in `app.json`). What happens to contacts after an
  uninstall, reinstall or device change therefore depends on the platform's
  own backup behaviour, which has not been verified for this project.
  Do not rely on it.
- The secret key backup described in [security.md](./security.md) does not
  include contacts.
- If saving to storage fails, the error is only logged, so a contact may
  appear in the list for the current session but not be saved.

## Future export / import considerations

These are ideas, not planned features:

- Export contacts as a JSON or CSV file through the system share sheet.
- Import from a file, reusing the existing address validation and duplicate
  detection, and deciding how to handle contacts with the same address.
- Handle `id` collisions on import, since ids are generated locally.
- Treat exports as personal data: warn users before sharing a file with
  names and addresses, and never log its contents.
- Decide explicitly whether OS-level device backups should include contacts.
