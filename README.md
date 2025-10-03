# Water Tracker — Transparent Desktop Overlay

A Unity-based desktop overlay to track daily water intake with visual and audio feedback for Windows.

---

## Purpose

The Water Tracker provides a simple, interactive way to monitor the daily water intake.  
The overlay displays **five bottles**, each representing 500ml, and allows you to add water using buttons:  

- **+1 Glass (250ml)**  
- **+1 Bottle (500ml)**  

An animated **milliliter counter** tracks total consumption, and sound effects provide feedback for every interaction.  

This project was inspired by [this YouTube tutorial](https://www.youtube.com/watch?v=x8BO9C6YtlE), which helped me with the transparency feature.

---

## Features

- Interactive **bottle and glass buttons** to add water  
- Animated **counter** for total intake  
- **Sound effects** on button presses  
- Clean **transparent overlay** for desktop  

---


## Assets & Licenses

This project uses the following third-party assets:  

### Pixel Art
| Asset | Source | Creator |
|-------|--------|---------|
| Bottles | [Free 2D Item 16x16 - Glass Bottles](https://jgiio.itch.io/free-16x16-glass-bottles) | JGIIO |
| Buttons | [Free Pixel Art Button Pack](https://ok-lavender.itch.io/free-pixel-art-button-pack) | ok_lavender |
| Font | [monogram](https://datagoblin.itch.io/monogram) | datagoblin |

### Audio
| Asset | Source | Creator |
|-------|--------|---------|
| Bottle Fill | [Pixabay](https://pixabay.com//?utm_source=link-attribution&utm_medium=referral&utm_campaign=music&utm_content=116975">Pixabay) | [Universfield](https://pixabay.com/users/universfield-28281460/?utm_source=link-attribution&utm_medium=referral&utm_campaign=music&utm_content=116975">Universfield)|

> Please refer to the respective asset pages for full license details.  

---

## Project Structure
Water_Tracker  
├─ Assets  
│ ├─ Audio  
│ │ └─ WaterSound  
│ ├─ Fonts  
│ │ └─ monogram  
│ ├─ Scenes  
│ │ ├─ SampleScene  
│ │ ├─ Start  
│ │ └─ WaterTracker  
│ ├─ Scripts  
│ │ ├─ BottleBehaviour  
│ │ ├─ CountMililiters  
│ │ ├─ MoveWindow  
│ │ ├─ NextScene  
│ │ └─ TransparentWindowManager  
│ ├─ Sprites  
│ │ ├─ Bottles.png  
│ │ ├─ Buttons.png  
│ │ └─ FilledBottle.png  
│ ├─ TextMeshPro  
├─ Packages  

---

## Visual Demonstration

### GIF Examples

- **Adding a Glass:**  
  [WaterTrackerVideo_Glass.gif](images/WaterTrackerVideo_Glass.gif)

- **Adding a Bottle:**  
  [WaterTrackerVideo_Bottle.gif](images/WaterTrackerVideo_Bottle.gif)

---

## Code Highlights

These snippets display certain functions of the Water Tracker project, especially **Animation and Unity-Windows-interaction**.

---

### 1️⃣ Transparent Overlay Setup (Windows Only)
Demonstrates how the Unity window is made transparent using **Windows API calls**:

```csharp
// Get the current window handle
var windowHandle = GetActiveWindow();

// Set window style to popup + visible
SetWindowLong(windowHandle, GWL_STYLE, WS_POPUP | WS_VISIBLE);

// Extend the frame into the client area for transparency, margins set to -1
DwmExtendFrameIntoClientArea(windowHandle, ref margins);
```

### 2️⃣ Animated Milliliter Counter
Shows smooth counter animation using coroutines, including handling multiple increments while counting:  

```csharp
public void StartCounting(int addValue)
{
    if (countCoroutine != null)
        StopCoroutine(countCoroutine);

    targetValue += addValue;
    countCoroutine = StartCoroutine(CountUpTo());
}

private IEnumerator CountUpTo()
{
    while (currentValue < targetValue)
    {
        currentValue += 10;
        counterText.text = currentValue.ToString();
        yield return new WaitForSeconds(delay);
    }
}
```

### 3️⃣ Bottle State Management
Manages each bottle’s fill state (empty, half-full, full) and propagates filling to the next bottle:  

```csharp
public void FillUp()
{
    if (isEmpty) { bottomFluid.SetActive(true); isHalfFull = true; }
    else if (isHalfFull) { topFluid.SetActive(true); isFull = true; }
    else if (nextBottle != null) { nextBottle.GetComponent<BottleBehaviour>().FillUp(); }
}
```

---

## License

[MIT](LICENSE) — free to use for personal, educational, or academic purposes.  

---

## Contact

- Name: Annalena Schmid
- Email: annalena.schmid.2003@gmail.com 
- GitHub: [github.com/AnnSchmid](https://github.com/AnnSchmid)
