using UnityEngine;
using System.Collections.Generic;

public class YutThrower : MonoBehaviour
{
    public GameObject yutPrefab;
    public int numberOfYut = 4;
    public float throwForce = 10f;
    public float torqueForce = 5f;
    public Transform spawnPoint;

    private List<GameObject> currentYuts = new List<GameObject>();

    void Update()
    {
        if (Input.GetKeyDown(KeyCode.Space))
        {
            ClearExistingYuts();
            ThrowYut();
        }
    }

    void ClearExistingYuts()
    {
        foreach (GameObject yut in currentYuts)
        {
            Destroy(yut);
        }
        currentYuts.Clear();
    }

    void ThrowYut()
    {
        for (int i = 0; i < numberOfYut; i++)
        {
            GameObject yut = Instantiate(yutPrefab, spawnPoint.position, Random.rotation);
            currentYuts.Add(yut);

            Rigidbody rb = yut.GetComponent<Rigidbody>();
            if (rb != null)
            {
                Vector3 throwDirection = (Vector3.up + spawnPoint.forward).normalized;
                rb.AddForce(throwDirection * throwForce, ForceMode.Impulse);
                rb.AddTorque(Random.insideUnitSphere * torqueForce, ForceMode.Impulse);
            }
        }
    }
}