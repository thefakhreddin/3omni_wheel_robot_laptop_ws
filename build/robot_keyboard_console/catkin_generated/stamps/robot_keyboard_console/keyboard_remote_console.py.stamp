import threading
import time
from pynput.keyboard import Key, Listener
from pynput import keyboard
import rospy
from geometry_msgs.msg import Twist

cmd_msg = Twist()

top_linear_speed = 0.5
top_angular_speed = 1.5

def main():
    rospy.init_node('keyboard_remote_console', anonymous=True)

    ros_thread = threading.Thread(target=ros_node_handler)
    ros_thread.start()

    with Listener(on_press=key_pressed, on_release=key_released) as listener:
            listener.join()

def key_pressed(key):
    try:
        if key.char == 'w':
            cmd_msg.linear.y = top_linear_speed
        if key.char == 's':
            cmd_msg.linear.y = -top_linear_speed
        if key.char == 'a':
            cmd_msg.linear.x = top_linear_speed
        if key.char == 'd':
            cmd_msg.linear.x = -top_linear_speed
        if key.char == 'q':
            cmd_msg.angular.z = top_angular_speed
        if key.char == 'e':
            cmd_msg.angular.z = -top_angular_speed
    except AttributeError:
        pass

def key_released(key):
    try:
        if key.char == 'w' or key.char == 's':
            cmd_msg.linear.y = 0
        if key.char == 'a' or key.char == 'd':
            cmd_msg.linear.x = 0
        if key.char == 'q' or key.char == 'e':
            cmd_msg.angular.z = 0
    except AttributeError:
        pass

def ros_node_handler():
    pub = rospy.Publisher('cmd_vel', Twist, queue_size=50)
    rate = rospy.Rate(5)
    while True:
        pub.publish(cmd_msg)
        rate.sleep()

main()