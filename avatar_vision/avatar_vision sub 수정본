import rclpy
from rclpy.node import Node
from sensor_msgs.msg import CompressedImage

import cv2
import numpy as np


class RealSenseRGBSubscriber(Node):
    def __init__(self):
        super().__init__('realsense_rgb_subscriber')

        self.subscription = self.create_subscription(
            CompressedImage,
            '/realsense/color/image_raw/compressed',
            self.callback,
            10
        )

        self.get_logger().info(
            'Subscribed to /realsense/color/image_raw/compressed'
        )

    def callback(self, msg):
        # 1️⃣ compressed byte → numpy array
        np_arr = np.frombuffer(msg.data, np.uint8)

        # 2️⃣ JPEG 디코딩
        frame = cv2.imdecode(np_arr, cv2.IMREAD_COLOR)
        if frame is None:
            self.get_logger().warn('Failed to decode image')
            return

        # 3️⃣ 영상 출력
        cv2.imshow('RealSense RGB (Compressed Subscriber)', frame)
        cv2.waitKey(1)


def main(args=None):
    rclpy.init(args=args)
    node = RealSenseRGBSubscriber()
    rclpy.spin(node)
    node.destroy_node()
    rclpy.shutdown()
    cv2.destroyAllWindows()


if __name__ == '__main__':
    main()
