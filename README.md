# AutoRewriter

[![NuGet Version](https://img.shields.io/nuget/v/MSJD.AutoReplacer.svg?style=flat&logo=nuget)](https://www.nuget.org/packages/MSJD.AutoReplacer)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

A Blazor component that automatically replaces words in real-time as the user types, with full support for validation, text area mode, and customization.
A Blazor component for **automatic text replacement** in `<input>` or `<textarea>` fields with **validation support**. It allows you to replace words on-the-fly while typing, enforce max length, and optionally switch between single-line and multi-line inputs.

---

## Features

- **Automatic text replacement** based on a dictionary of words.
- **Ignore case** option for replacements.
- **Max length enforcement**.
- **Supports validation** using `DataAnnotations` and `ValidationMessage`.
- **Switchable between input and textarea** using `IsTextArea`.
- **Customizable placeholder, CSS class, and disabled state**.

---

## Installation

Add the NuGet package to your Blazor project:

```bash
dotnet add package MSJD.AutoRewriter --version 1.0.0
```
## Usage

### Basic Example (Input)

```razor
@page "/auto-rewriter-sample"
@using MyCompany.AutoRewriter

<EditForm Model="formData" OnValidSubmit="HandleSubmit">
    <DataAnnotationsValidator />

    <AutoRewriter
        @bind-Value="formData.Comment"
        ValidationFor="() => formData.Comment"
        ReplacementsItems="wordMap"
        Placeholder="Type a comment..."
        Class="form-control" />

    <button type="submit" class="btn btn-primary mt-2">Submit</button>
</EditForm>

<p>Filtered comment: @formData.Comment</p>

@code {
    private FormData formData = new();
    private Dictionary<string, string> wordMap = new()
    {
        ["badword"] = "goodword",
        ["oops"] = "ok"
    };

    private class FormData
    {
        [Required(ErrorMessage = "Comment cannot be empty.")]
        public string Comment { get; set; } = string.Empty;
    }

    private void HandleSubmit()
    {
        // Form submission logic
        Console.WriteLine($"Submitted comment: {formData.Comment}");
    }
}
```

### Multi-line Example (Textarea)

```razor
<AutoRewriter
    @bind-Value="formData.Description"
    ValidationFor="() => formData.Description"
    ReplacementsItems="wordMap"
    IsTextArea="true"
    Placeholder="Enter detailed description..."
    Class="form-control"
    MaxLength="500" />
```

## Parameters

| Parameter           | Type                        | Default               | Description                                      |
|--------------------|-----------------------------|---------------------|--------------------------------------------------|
| `ValidationFor`     | `Expression<Func<string>>`  | `required`          | Property for validation message.                |
| `ReplacementsItems` | `Dictionary<string,string>` | `required`          | Dictionary of word replacements.               |
| `IgnoreCase`        | `bool`                      | `true`              | Case-insensitive replacement.                   |
| `Placeholder`       | `string`                    | `"Enter your text"` | Placeholder text.                               |
| `Class`             | `string`                    | `"form-control"`    | CSS class for input/textarea.                  |
| `MaxLength`         | `int`                       | `1000`              | Maximum number of characters allowed.          |
| `IsDisabled`        | `bool`                      | `false`             | Disable input/textarea.                        |
| `IsTextArea`        | `bool`                      | `false`             | Render a `<textarea>` instead of `<input>`.    |

---

## How It Works

- Integrates with **Blazor forms and validation** using `InputBase<string>`.  
- Automatically **replaces words** from `ReplacementsItems` as the user types.  
- Enforces **maximum length** (`MaxLength`).  
- Displays **validation messages** using `ValidationMessage`.  
- Supports both **single-line input** and **multi-line textarea** with `IsTextArea`.

---

## Example Replacements

| Input     | Output    |
|-----------|----------|
| `badword` | `goodword` |
| `oops`    | `ok`      |

---

## Notes

- Always bind the component to a **model property** for proper validation.  
- Use `IsTextArea="true"` for multi-line content.  
- Customize **placeholder**, **CSS class**, and **MaxLength**.  
- Works in **Blazor Server** and **WebAssembly** projects.  

---

## License

MIT License – see LICENSE file.
