![[WatermelonGame.png]]
![[WatermelonGame2.png]]

# Inspectors
![[WatermelonGame_Inspectors.png]]
#### GameManager
![[WatermelonGame_GameManager.png]]

#### Dongle
![[WatermelonGame_Dongle.png]]

#### Effect
![[WatermelonGame_Effect.png]]

# Animation
![[WatermelonGame_Animation.png]]

# Animator
![[WatermelonGame_Animator.png]]

# Scripts
#### GameManager
```cs
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.SceneManagement;
using UnityEngine.UI;

public class GameManager : MonoBehaviour
{
    public static GameManager Instance;
    public Dongle lastDongle;
    public GameObject donglePref;
    public Transform dongleGroup;
    public GameObject particlePref;
    public Transform particleGroup;
    public int maxLevel;
    bool isGameOver = false;

    public AudioSource bgmPlayer;
    public AudioSource[] effectPlayer;
    public AudioClip[] audioClips;
    int effectPlayerIdx = 0;

    public int score = 0;
    public Text scoreText;

    public GameObject menuGameOver;
    public Text buttonScoreText;

    public enum SFX
    {
        level,
        next,
        attach,
        button,
        gameover
    }
    Dictionary<SFX, float> dictSfxTime = new Dictionary<SFX, float>();

    private void Start()
    {
        NextDongle();
        maxLevel = 2;
        bgmPlayer.Play();
        scoreText.text = "0";
        menuGameOver.SetActive(false);
    }

    private void Awake()
    {
        Instance = this;
    }

    void NextDongle()
    {
        if (isGameOver)
        {
            return;
        }
        lastDongle = GetDongle();
        lastDongle.level = Random.Range(0, maxLevel);
        lastDongle.gameObject.SetActive(true);
        SFXPlay(SFX.next);
    }

    public Dongle GetDongle()
    {
        GameObject particle = Instantiate(particlePref, particleGroup);

        GameObject clone = Instantiate(donglePref, dongleGroup);
        Dongle dongle = clone.GetComponent<Dongle>();
        dongle.effect = particle.GetComponent<ParticleSystem>();
        return dongle;
    }

    public void TouchDown()
    {
        if (lastDongle == null)
        {
            return;
        }
        lastDongle.MouseDrag();
    }

    public void TouchUp()
    {
        if (lastDongle == null)
        {
            return;
        }
        lastDongle.MouseDrop();
        lastDongle = null;
        Invoke("NextDongle", 2.5f);
    }

    public void GameOver()
    {
        if (isGameOver)
        {
            return;
        }
        isGameOver = true;
        bgmPlayer.Stop();

        SFXPlay(SFX.gameover);
        StartCoroutine("DoGameOver");
    }

    IEnumerator DoGameOver()
    {
        Dongle[] dongles = FindObjectsOfType<Dongle>();
        foreach (Dongle dongle in dongles)
        {
            dongle.Hide();
            yield return new WaitForSeconds(0.1f);
        }
        ShowGameOverMenu();
    }

    public void SFXPlay(SFX sfx)
    {
        if (dictSfxTime.ContainsKey(sfx) && (Time.time - dictSfxTime[sfx]) < 0.2f)
        {
            return;
        }

        switch (sfx)
        {
            case SFX.level:
                effectPlayer[effectPlayerIdx].clip = audioClips[Random.Range(0, 3)];
                break;
            case SFX.next:
                effectPlayer[effectPlayerIdx].clip = audioClips[3];
                break;
            case SFX.attach:
                effectPlayer[effectPlayerIdx].clip = audioClips[4];
                break;
            case SFX.button:
                effectPlayer[effectPlayerIdx].clip = audioClips[5];
                break;
            default:
                effectPlayer[effectPlayerIdx].clip = audioClips[6];
                break;
        }
        effectPlayer[effectPlayerIdx].Play();
        effectPlayerIdx = (effectPlayerIdx + 1) % effectPlayer.Length;
        dictSfxTime[sfx] = Time.time;
    }

    public void AddScore(int score)
    {
        this.score += score;
        scoreText.text = this.score + "";
    }

    void ShowGameOverMenu()
    {
        menuGameOver.SetActive(true);
        buttonScoreText.text = "점수 : " + score;
    }

    public void ButtonAct_Restart()
    {
        SceneManager.LoadScene(0);
    }

}
```

#### Dongle
```cs
using JetBrains.Annotations;
using System;
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using static UnityEditor.PlayerSettings;

public class Dongle : MonoBehaviour
{
    Transform myTF;
    Rigidbody2D rd;
    bool isMouseDrag;
    public int level;
    Animator animator;
    public bool isMerge;
    CircleCollider2D circleCollider;
    public ParticleSystem effect;
    float deadTime;
    SpriteRenderer spriteRenderer;

    private void Awake()
    {
        myTF = transform;
        rd = GetComponent<Rigidbody2D>();
        animator = GetComponent<Animator>();
        circleCollider = GetComponent<CircleCollider2D>();
        spriteRenderer = GetComponent<SpriteRenderer>();
    }

    private void OnEnable()
    {
        animator.SetInteger("Level", level);
    }

    private void Update()
    {
        if (isMouseDrag)
        {
            Vector3 mousePos = Camera.main.ScreenToWorldPoint(Input.mousePosition);

            float radius = myTF.localScale.x * 0.5f;
            mousePos.x = Mathf.Clamp(mousePos.x, -4f + radius, 4f - radius);
            mousePos.y = 7.5f;
            mousePos.z = 0;

            myTF.position = Vector3.Lerp(mousePos, myTF.position, 0.1f);
        }
    }

    public void MouseDrag()
    {
        isMouseDrag = true;
    }

    public void MouseDrop()
    {
        isMouseDrag = false;
        rd.simulated = true;
    }

    public bool CanMerge(Dongle dongle)
    {
        return level == dongle.level && isMerge == false && dongle.isMerge == false && level < 7;
    }

    private void OnCollisionEnter2D(Collision2D collision)
    {
        if ( collision.gameObject.tag.Equals("Dongle") )
        {
            GameManager.Instance.SFXPlay(GameManager.SFX.attach);
        }
    }

    private void OnCollisionStay2D(Collision2D collision)
    {
        if ( collision.gameObject.tag.Equals("Dongle") )
        {
            Dongle dongle = collision.gameObject.GetComponent<Dongle>();
            if (dongle == null )
            {
                return;
            }
            if ( CanMerge(dongle) )
            {
                float myX = myTF.position.x;
                float myY = myTF.position.y;
                float x = dongle.myTF.position.x;
                float y = dongle.myTF.position.y;
                if (myY < y || (myY < y && myX > x) )
                {
                    dongle.Hide(myTF.position);
                    LevelUp();
                }
            }
        }
    }

    private void OnTriggerStay2D(Collider2D collision)
    {
        if ( collision.tag.Equals("Finish") )
        {
            deadTime += Time.deltaTime;
            if (deadTime > 2f)
            {
                spriteRenderer.color = Color.red;
            }
            if (deadTime > 5f)
            {
                GameManager.Instance.GameOver();
            }
        }
    }

    private void OnTriggerExit2D(Collider2D collision)
    {
        if ( collision.tag.Equals("Finish") )
        {
            deadTime = 0;
            spriteRenderer.color = Color.white;
        }
    }

    public void Hide(Vector3 pos)
    {
        isMerge = true;
        rd.simulated = false;
        circleCollider.enabled = false;
        animator.enabled = false;
        StartCoroutine( DoHide(pos) );
    }
    public void Hide()
    {
        if (rd.simulated == true)
        {
            GameManager.Instance.AddScore( (int)Math.Pow(2, level) );
        }

        isMerge = true;
        rd.simulated = false;
        circleCollider.enabled = false;
        animator.enabled = false;
        StartCoroutine( DoHide() );
    }

    IEnumerator DoHide(Vector3 pos) //Coroutine
    {
        int frameCount = 0;
        while (frameCount < 20)
        {
            frameCount++;
            myTF.position = Vector3.Lerp(myTF.position, pos, 0.5f);
            yield return null;
        }
        isMerge = false;
        gameObject.SetActive(false);
    }

    IEnumerator DoHide() //Coroutine
    {
        int frameCount = 0;
        while (frameCount < 20)
        {
            frameCount++;
            myTF.localScale = Vector3.Lerp(myTF.localScale, Vector3.zero, 0.2f);
            yield return new WaitForSeconds(0.03f);
        }
        isMerge = false;
        gameObject.SetActive(false);
    }

    void LevelUp()
    {
        isMerge = true;
        rd.velocity = Vector2.zero;
        rd.angularVelocity = 0;
        StartCoroutine( DoLevelUp() );
    }

    IEnumerator DoLevelUp() //Coroutine
    {
        yield return new WaitForSeconds(0.2f);
        animator.SetInteger("Level", ++level);
        GameManager.Instance.AddScore( (int)Math.Pow(2, level) );

        GameManager.Instance.maxLevel = Math.Max(level, GameManager.Instance.maxLevel);

        PlayEffect(); //Particle
        GameManager.Instance.SFXPlay(GameManager.SFX.level);

        yield return new WaitForSeconds(0.3f);
        isMerge = false;
    }
    
    void PlayEffect()
    {
        effect.transform.position = myTF.position;
        effect.transform.localScale = myTF.localScale;
        effect.Play();
    }
}
```