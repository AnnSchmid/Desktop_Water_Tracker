using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class NewBehaviourScript : MonoBehaviour
{
    // Start is called before the first frame update
    void Start()
    {
        Display.main.Activate(0, 0, new RefreshRate() { numerator = 60, denominator = 1 });
    }

    // Update is called once per frame
    void Update()
    {
        Display.main.SetParams(50, 50, 0, 0);
    }
}
