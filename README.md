using UnityEngine;
using System.Collections;

// A class to represent a football player
public class Player : MonoBehaviour
{
    // The speed of the player
    public float speed = 10f;

    // The force of the player's kick
    public float kickForce = 20f;

    // The rigidbody component of the player
    private Rigidbody rb;

    // The animator component of the player
    private Animator anim;

    // The input axis for horizontal movement
    private float horizontal;

    // The input axis for vertical movement
    private float vertical;

    // The input button for kicking the ball
    private bool kick;

    // The ball object
    public GameObject ball;

    // The distance from the ball to kick it
    public float kickDistance = 2f;

    // The direction of the kick
    private Vector3 kickDirection;

    // Start is called before the first frame update
    void Start()
    {
        // Get the rigidbody component
        rb = GetComponent<Rigidbody>();

        // Get the animator component
        anim = GetComponent<Animator>();
    }

    // Update is called once per frame
    void Update()
    {
        // Get the input values from the keyboard or controller
        horizontal = Input.GetAxis("Horizontal");
        vertical = Input.GetAxis("Vertical");
        kick = Input.GetButtonDown("Fire1");

        // Set the animator parameters based on the input values
        anim.SetFloat("Speed", Mathf.Abs(horizontal) + Mathf.Abs(vertical));
        anim.SetBool("Kick", kick);

        // Calculate the direction of the kick based on the position of the ball and the player
        kickDirection = (ball.transform.position - transform.position).normalized;
    }

    // FixedUpdate is called once per physics update
    void FixedUpdate()
    {
        // Move the player based on the input values and the speed
        rb.velocity = new Vector3(horizontal, 0, vertical) * speed;

        // Rotate the player to face the direction of movement
        if (rb.velocity != Vector3.zero)
        {
            transform.rotation = Quaternion.LookRotation(rb.velocity);
        }

        // If the player presses the kick button and is close enough to the ball, kick it with a force
        if (kick && Vector3.Distance(transform.position, ball.transform.position) < kickDistance)
        {
            ball.GetComponent<Rigidbody>().AddForce(kickDirection * kickForce, ForceMode.Impulse);
        }
    }
}
