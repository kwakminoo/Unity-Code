GameManager.cs
-----------
~~~C#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using TMPro;
using UnityEngine.SceneManagement;
using UnityEngine.UI;

public class GameManager : MonoBehaviour
{
    //타겟이란 변수를 리시트로 묶음
    public List<GameObject> targets;
    public float SpawnRate = 1.0f;
    private int score;
    public TextMeshProUGUI scoreText;
    public TextMeshProUGUI gameOverText;
    public bool isGameActive;
    public Button restartButton;
    public GameObject titleScreen;

    //int difficulty로 이미 연동돼있는 difficultyButon.cs의 변수를 가져옴
    public void StartGame(int difficulty)
    {
        isGameActive = true;
        StartCoroutine(SpawnTarget());
        SpawnRate /= difficulty;
        score = 0;
        UpdateScore(0);
        titleScreen.SetActive(false);
    }

    public void RestartGame()
    {
        //씬을 현제씬의 이름으로 넘어가겠다
        SceneManager.LoadScene(SceneManager.GetActiveScene().name);
    }

    //새로운 지역변수를 만들고 그걸로 스커오를 카운트
    public void UpdateScore(int scoreToAdd)
    {
        score += scoreToAdd;
        scoreText.text = "Score: " + score; 
    }

    public void GameOver()
    {
        gameOverText.gameObject.SetActive(true);
        restartButton.gameObject.SetActive(true);
        isGameActive = false;
    }

    IEnumerator SpawnTarget()
    {
        //while은 특정 조건일 때 반복하는 메소드로 isGameAcrive가 ture일 때만 실행됨
        while(isGameActive)
        {
            yield return new WaitForSeconds(SpawnRate);
            int index = Random.Range(0, targets.Count);
            Instantiate(targets[index]);
        }
    } 
}
~~~

difficultyButton.cs
------
~~~C#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.UI;

public class DiffcultyButton : MonoBehaviour
{
    private Button button;
    private GameManager gameManager;
    public int difficulty;

    void Start()
    {
        //button이라는 함수에 Button컴포넌트를 사용
        button = GetComponent<Button>();
        gameManager = GameObject.Find("GameManager").GetComponent<GameManager>();
        //button 함수가 눌리면 눌린 기점을 기억
        button.onClick.AddListener(SetDifficulty);
    }

    //button이 눌렸을 때 잘 작동하나 확인하고 GameManager.cs의 StartGame변수를 가져옴
    void SetDifficulty()
    {
        Debug.Log(gameObject.name + "is Click");
        gameManager.StartGame(difficulty);
    }

    // Update is called once per frame
    void Update()
    {
        
    }
}

~~~

Target.cs
-----------
~~~C#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Target : MonoBehaviour
{
    private Rigidbody targetRb;

    public float maxSpeed = 16;
    public float minSpeed = 12;
    public float maxTorque = 10;
    public float xRange = 4;
    public float ySpawnPos = -6;
    private GameManager gameManager;
    public int pointValue;
    public ParticleSystem explosionParticle;

    void Start()
    {
        targetRb = GetComponent<Rigidbody>();
        //Force는 물체에 가해지는 '힘' Torque는 물체에 가해지는 '회전'을 나타냄
        targetRb.AddForce(RandomForce() , ForceMode.Impulse);
        targetRb.AddTorque(RandomTorque(), RandomTorque(), RandomTorque(), ForceMode.Impulse);
        transform.position = RandomSpawnPos();
        gameManager = GameObject.Find("GameManager").GetComponent<GameManager>();
    }

    Vector3 RandomForce()
    {
        return Vector3.up * Random.Range(minSpeed, maxSpeed);
    }

    float RandomTorque()
    {
        return Random.Range(-maxTorque, maxTorque);
    }

    Vector3 RandomSpawnPos()
    {
        return new Vector3(Random.Range(-xRange, xRange), ySpawnPos, 0);
    }

    private void OnMouseDown()
    {
        if(gameManager.isGameActive)
        {
            Destroy(gameObject);
            gameManager.UpdateScore(pointValue);
            Instantiate(explosionParticle, transform.position, explosionParticle.transform.rotation);
        }
        
    }

    //콜라이더가 충돌하면 게임 오브젝트를 삭제한다 이때 오브젝트의 태그가 Bad일때는 삭제하지 않는다 
    private void OnTriggerEnter(Collider other)
    {
        Destroy(gameObject);
        if (!gameObject.CompareTag("Bad"))
        {
            gameManager.GameOver();
        }
    }
}
~~~
