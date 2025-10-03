```csharp
using System.Collections;
using UnityEngine;
using TMPro;

public class CountMililiters : MonoBehaviour
{
    public float delay = 0.1f;
    private int currentValue = 0;
    private int targetValue = 0;
    public TextMeshProUGUI counterText;

    private Coroutine countCoroutine;

    void Start()
    {
        counterText.text = currentValue.ToString();
    }

    public void StartCounting(int addValue)
    {
        // Stop old coroutine, but preserve progress
        if (countCoroutine != null)
            StopCoroutine(countCoroutine);

        // Increase target by the new increment
        targetValue += addValue;

        // Start counting up to the new target
        countCoroutine = StartCoroutine(CountUpTo());
    }

    private IEnumerator CountUpTo()
    {
        while (currentValue < targetValue)
        {
            currentValue = currentValue +10;
            counterText.text = currentValue.ToString();
            yield return new WaitForSeconds(delay);
        }

        countCoroutine = null; // reset when done
    }
}

```
