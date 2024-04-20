#3D 
![[BallController.png]]
# Inspectors
#### Ball
![[BallController_Ball.png]]

#### Floor
![[BallController_Floor.png]]

#### Wall
![[BallController_Wall.png]]

#### PickUp
![[BallController_PickUp.png]]

#### Score Text
![[BallController_ScoreText.png]]

#### Clear Text
![[BallController_ClearText.png]]

# Codes
#### PlayerController
```cs
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.UI;

public class PlayerController : MonoBehaviour
{
    public float speed;
    public Text _scoreText;
    public Text _clearText;
    int _score;
    int _count;

    private void Awake()
    {
        _score = 0;
        _count = 0;
        _clearText.text = "";
    }

    // Start is called before the first frame update
    void Start() //게임이 실행되었을 때 모든 오브젝트를 대상으로 호출 | Awake() 함수가 먼저 호출된 후, Start()가 호출됨
    {
    }

    private void FixedUpdate()
    {
        float moveHorizontal = Input.GetAxis("Horizontal"); //미입력시 0, 입력시 -1 ~ 1 사이의 실수가 들어옴.
        //float moveHorizontal = (Input.GetKey("d") ? 1.0f : 0f) - (Input.GetKey("a") ? 1.0f : 0f);
        float moveVertical = Input.GetAxis("Vertical");
        Rigidbody rb = GetComponent<Rigidbody>();
        rb.AddForce(new Vector3(moveHorizontal, 0, moveVertical) * speed);

        //Stop when no inputs
        if (moveHorizontal == 0 && moveVertical == 0)
        {
            rb.AddForce(rb.velocity * -0.6f * speed);
        }
    }

    //private void OnCollisionEnter(Collision collision) { }
    //private void OnCollisionExit(Collision collision) { }
    //private void OnCollisionStay(Collision collision) { }

    private void OnTriggerEnter(Collider other)
    {
        if ( other.tag.Equals("PickUp") )
        {
            Destroy(other.gameObject); //충돌한 오브젝트 삭제
            _score++;
            _scoreText.text = "Score : " + _score.ToString();
            _count++;
            if (_count >= 4)
            {
                _clearText.text = "Game Clear!";
            }
        }
    }

    //private void OnTriggerExit(Collider other) { }
    //private void OnTriggerStay(Collider other) { }
}
```

#### PickUpController
```cs
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.UIElements;

public class PickUpController : MonoBehaviour
{
    public Vector3 _speed;
    private void Update()
    {
        transform.Rotate( _speed * Time.deltaTime );
    }

    private void OnTriggerEnter(Collider other)
    {
        if ( other.tag.Equals("Player") )
        {
            transform.position = new Vector3( Random.Range(-9, 9), 0.5f, Random.Range(-9, 9) );
        }
    }
}
```

#### CameraController
```cs
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class CameraController : MonoBehaviour
{
    public Transform _player;
    public Vector3 _offset;

    public void Start()
    {
        _offset = _player.position - transform.position;
        transform.LookAt(_player);
    }

    public void LateUpdate()
    {
        transform.position = _player.position - _offset;
    }
}
```