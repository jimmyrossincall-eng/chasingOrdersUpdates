# How to Add New Braces (Non-Coders Guide)

## 🎯 Overview

The app is now **fully dynamic**! You can add new brace types, codes, and categories just by editing the JSON file. **No coding knowledge required!**

## 📝 Adding a New Brace Code to Existing Category

### Example: Add a new Back brace code

1. Open `config/braces_data.json`
2. Find the "Back" section
3. Add your new code following this pattern:

```json
"Back": {
  "L0637": [
    "Description of the brace",
    "Diagnosis code 1",
    "Diagnosis code 2"
  ],
  "L9999": [              ← NEW CODE
    "Your new brace description",
    "First diagnosis (M12.34)",
    "Second diagnosis (M56.78)",
    "Third diagnosis (M90.12)"
  ]
}
```

4. **IMPORTANT:** Update the version number at the top:
```json
"version": "1.1",    ← Change this!
```

5. Save and commit to GitHub
6. **Done!** The app will pick it up automatically!

---

## 🆕 Adding a Completely New Brace Category

### Example: Add "Foot" braces

#### Step 1: Add to braces_list

```json
{
  "version": "1.1",
  "last_updated": "2025-12-08",
  "braces_list": [
    "Back", 
    "Knees", 
    "Elbow", 
    "Shoulder", 
    "Ankle", 
    "Wrists", 
    "Neck", 
    "Hip",
    "Foot"        ← ADD HERE
  ],
```

#### Step 2: Add metadata

Decide if it's bilateral (left/right) or single-sided:

```json
"brace_metadata": {
  "Back": {"bilateral": false, "label": "Back"},
  "Knees": {"bilateral": true, "label": "Knee"},
  ...
  "Foot": {"bilateral": true, "label": "Foot"}    ← ADD HERE
},
```

**Options:**
- `"bilateral": true` → Shows "Left Foot" and "Right Foot" checkboxes
- `"bilateral": false` → Shows only "Foot" checkbox
- `"label"` → The display name (can be different from the key)

#### Step 3: Add brace codes

```json
"brace_info": {
  "Back": { ... },
  "Knees": { ... },
  ...
  "Foot": {                              ← NEW CATEGORY
    "F1234": [
      "Foot orthosis description",
      "Diagnosis code 1",
      "Diagnosis code 2"
    ],
    "F5678": [
      "Another foot brace",
      "Diagnosis code 1",
      "Diagnosis code 2"
    ]
  }
}
```

#### Step 4: Create template (optional)

If you want a custom form template:

1. Create `templates/foot.html` (copy from `templates/back.html` as a starting point)
2. Modify as needed
3. Upload to GitHub

#### Step 5: Update version and save

```json
"version": "1.2",    ← Increment!
"last_updated": "2025-12-08"
```

**That's it!** The app will:
- ✅ Show "Foot" in the braces list
- ✅ Show all F-codes you added
- ✅ Generate correct checkboxes (Left/Right if bilateral)
- ✅ Use the template if you created one

---

## 📋 Complete Example: Adding Leg Braces

Here's a full example of adding a new "Leg" category:

```json
{
  "version": "1.3",
  "last_updated": "2025-12-08",
  
  "braces_list": [
    "Back", "Knees", "Elbow", "Shoulder", 
    "Ankle", "Wrists", "Neck", "Hip", 
    "Leg"                                    ← STEP 1: Add to list
  ],
  
  "brace_metadata": {
    "Back": {"bilateral": false, "label": "Back"},
    "Knees": {"bilateral": true, "label": "Knee"},
    ...
    "Leg": {"bilateral": true, "label": "Leg"}    ← STEP 2: Add metadata
  },
  
  "brace_info": {
    "Back": { ... },
    "Knees": { ... },
    ...
    "Leg": {                                 ← STEP 3: Add codes
      "L1234": [
        "LEG ORTHOSIS, FULL LENGTH, CUSTOM FABRICATED",
        "Leg length discrepancy (M21.759)",
        "Weakness of leg (M62.81)",
        "Other instability, leg (M25.36)"
      ],
      "L5678": [
        "LEG ORTHOSIS, SHORT LENGTH, PREFABRICATED",
        "Leg pain (M79.604)",
        "Muscle strain, leg (S86.119A)"
      ]
    }
  }
}
```

**Result:** 
- Leg braces appear in the app automatically
- Two codes available: L1234 and L5678
- Shows "Left Leg" and "Right Leg" checkboxes
- Works immediately without any code changes!

---

## 🔍 Understanding the Structure

### Anatomy of a Brace Entry

```json
"CategoryName": {          ← Category (e.g., "Back", "Knees")
  "L1234": [              ← Brace Code (HCPCS/L-code)
    "Description",        ← [0] Full brace description
    "Diagnosis 1",        ← [1] First diagnosis code
    "Diagnosis 2",        ← [2] Second diagnosis code
    "Diagnosis 3",        ← [3] Third diagnosis code
    ...                   ← Add as many as you need
  ]
}
```

### Rules

✅ **DO:**
- Keep the structure exactly as shown
- Use double quotes `"` for strings
- Separate items with commas
- Update version number when making changes
- Test JSON syntax at [jsonlint.com](https://jsonlint.com)

❌ **DON'T:**
- Mix single `'` and double `"` quotes
- Forget commas between items
- Forget to update version number
- Use special characters without escaping

---

## 🧪 Testing Your Changes

### Before Pushing to GitHub:

1. **Validate JSON:**
   - Copy your entire JSON
   - Go to [jsonlint.com](https://jsonlint.com)
   - Paste and click "Validate JSON"
   - Fix any errors shown

2. **Test Locally (if you have Python):**
   ```bash
   python test_config.py
   ```
   Should show:
   ```
   ✓ Configuration loaded successfully
   ✓ Found X brace types
   ```

3. **Visual Check:**
   - Make sure all `{` have matching `}`
   - Make sure all `[` have matching `]`
   - Make sure commas are between items (but not after the last item)

### After Pushing:

1. Users will see update message within 24 hours
2. They restart the app
3. New braces appear automatically!

---

## 💡 Common Patterns

### Adding Multiple Codes at Once

```json
"Hip": {
  "L1653": [...],
  "L1690": [...],
  "L1700": [        ← NEW
    "Description",
    "Diagnosis"
  ],
  "L1710": [        ← NEW
    "Description",
    "Diagnosis"
  ]
}
```

### Creating a Single-Sided Brace

```json
"brace_metadata": {
  "Torso": {
    "bilateral": false,    ← Single checkbox
    "label": "Torso"
  }
}
```

### Creating a Bilateral Brace

```json
"brace_metadata": {
  "Hand": {
    "bilateral": true,     ← Left/Right checkboxes
    "label": "Hand"
  }
}
```

---

## 🚨 Troubleshooting

### "Invalid JSON" Error

**Problem:** Syntax error in JSON file

**Solution:**
1. Go to [jsonlint.com](https://jsonlint.com)
2. Paste your JSON
3. Fix the errors it shows (usually missing comma or quote)

### New Brace Not Showing

**Problem:** Forgot to add to `braces_list`

**Solution:** 
```json
"braces_list": ["Back", "Knees", ..., "YourNewBrace"]
```

### Checkboxes Look Wrong

**Problem:** Incorrect `bilateral` setting

**Solution:**
- For body parts with left/right: `"bilateral": true`
- For central body parts: `"bilateral": false`

### App Won't Update

**Problem:** Version number not changed

**Solution:** Always increment:
```json
"version": "1.0"  →  "version": "1.1"
```

---

## 📚 Real-World Workflow

### Scenario: Adding 3 new Knee codes

1. Open `config/braces_data.json` on GitHub
2. Find the "Knees" section
3. Add your codes:
   ```json
   "Knees": {
     "L1843": [...],
     "L1852": [...],
     "L1833": [...],
     "L9001": [        ← NEW
       "New knee brace type 1",
       "Diagnosis"
     ],
     "L9002": [        ← NEW
       "New knee brace type 2",
       "Diagnosis"
     ],
     "L9003": [        ← NEW
       "New knee brace type 3",
       "Diagnosis"
     ]
   }
   ```
4. Update version: `"1.0"` → `"1.1"`
5. Click "Commit changes"
6. **Done!** Takes 2 minutes, no coding!

---

## ✅ Checklist for Adding New Braces

- [ ] Added to `braces_list` (if new category)
- [ ] Added to `brace_metadata` (if new category)
- [ ] Added codes to `brace_info`
- [ ] Set correct `bilateral` value
- [ ] Used correct label
- [ ] Added all diagnoses
- [ ] Validated JSON at jsonlint.com
- [ ] Updated version number
- [ ] Saved/committed changes

---

## 🎉 Summary

**Before:** Had to edit Python code, understand if/elif chains, rebuild app

**Now:** Just edit JSON file following the pattern, push to GitHub, done!

**The app automatically:**
- ✅ Reads all brace types
- ✅ Reads all brace codes
- ✅ Generates correct checkboxes
- ✅ Uses correct templates
- ✅ Works with any structure you add

**You only need to:**
1. Follow the JSON pattern
2. Update version number
3. Push to GitHub

**No coding knowledge required!** 🚀

