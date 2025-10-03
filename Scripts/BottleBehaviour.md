```csharp
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class BottleBehaviour : MonoBehaviour
{
    private Animator anim;
    private bool isEmpty = true;
    private bool isHalfFull = false;
    private bool isFull = false;

    public GameObject bottomFluid, topFluid, nextBottle;
   
    void Start()
    {
        anim = this.GetComponent<Animator>();
    }

    public void FillUp()
    {
        if (isEmpty)
        {
            bottomFluid.SetActive(true);
            isEmpty = false;
            isFull = false;
            isHalfFull = true;
        }
        else if (isHalfFull)
        {
            topFluid.SetActive(true);
            isEmpty = false;
            isHalfFull = false;
            isFull = true;
        }
        else if (nextBottle != null)
        {
            nextBottle.GetComponent<BottleBehaviour>().FillUp();
        }
    }
}
```
