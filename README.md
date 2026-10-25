# Memair Data Types

The types Memair records are chosen from: biometrics, digital activities, emotions, journals, medication forms, physical activities and recommendations. Memair loads these files with [FrozenRecord](https://github.com/byroot/frozen_record), and its GraphQL API serves them.

Please submit a [PR](https://github.com/memair/data-types/pull/new/master) if you would like a new data type added. A type's `id` is stored with every record of that type, so an `id` never changes and is never reused.

## Apple Health

A type with a `healthkit` key is one the Memair iPhone app can import from Apple Health. The app reads these mappings from the API, so a new mapping reaches it without an app release, as long as it uses a kind the app already imports.

| File | Key | Meaning |
| --- | --- | --- |
| `biometric_types.yml` | `quantity` | The [`HKQuantityTypeIdentifier`](https://developer.apple.com/documentation/healthkit/hkquantitytypeidentifier) raw value. |
| | `unit` | The [`HKUnit`](https://developer.apple.com/documentation/healthkit/hkunit) string the value is read in. It must give the value in the type's `unit_symbol`. |
| | `multiplier` | What the value is multiplied by. HealthKit gives percentages as fractions, so percentages use 100. |
| `physical_activity_types.yml` | `workouts` | [`HKWorkoutActivityType`](https://developer.apple.com/documentation/healthkit/hkworkoutactivitytype) raw values. |
| | `category`, `values` | An [`HKCategoryTypeIdentifier`](https://developer.apple.com/documentation/healthkit/hkcategorytypeidentifier) raw value, and the category values that mean this activity. Without `values`, every sample of the category does. |
| `emotion_types.yml` | `state_of_mind_labels` | [`HKStateOfMind.Label`](https://developer.apple.com/documentation/healthkit/hkstateofmind/label) raw values. |
| `medication_forms.yml` | `general_forms` | [`HKMedicationGeneralForm`](https://developer.apple.com/documentation/healthkit/hkmedicationgeneralform) case names. |

Each Apple value belongs to one type at most. A cumulative quantity, such as steps, is imported as one record per day, so the types it maps to are daily totals. Every workout activity type and every State of Mind label that the iOS 26.2 SDK defines has a type. Where Apple's medication form does not say how the medication is taken, the mapping takes the common case: drops are eye drops, an injection is subcutaneous, and a spray is nasal.

An Android app would add a `healthconnect` key the same way.
