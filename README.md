- 👋(function(){
    var triggerDistance = 0.0;
    var TRIGGER_THRESHOLD = 0.9;
    var LOAD_THRESHOLD = 0.6
    var init = false;
    var rightHandIndex = MyAvatar.getJointIndex("RightHand");
    var rightArmIndex = MyAvatar.getJointIndex("RightArm");
    var distance = 0.0;
    var triggered = false;
    function fireBall(position, speed) {
        var baseID = Entities.addEntity({
            type: "Sphere",
            color: { blue: 128, green: 128, red: 20 },
            dimensions: { x: 0.1, y: 0.1, z: 0.1 },
            position: position,
            dynamic: true,
            collisionless: false,
            lifetime: 10,
            gravity: speed,
            userData: "{ \"grabbableKey\": { \"grabbable\": true, \"kinematic\": false } }"
        }); 
        Entities.editEntity(baseID, { velocity: speed });
    }
    Script.update.connect(function() {
        rightHandPos = MyAvatar.getJointPosition(rightHandIndex);
        rightArmPos = MyAvatar.getJointPosition(rightArmIndex);
        fireDir = Vec3.subtract(rightHandPos, rightArmPos);
        var distance = Vec3.length(fireDir);
        triggerDistance = distance > triggerDistance ? distance : triggerDistance;
        if (!triggered) {
            if (distance < LOAD_THRESHOLD * triggerDistance) {
                triggered = true;
            }
        } else if (distance > TRIGGER_THRESHOLD * triggerDistance) {
            triggered = false;
            fireBall(rightHandPos, Vec3.normalize(fireDir));
        }     
    });
    MyAvatar.scaleChanged.connect(function () {
        triggerDistance = 0.0;
    });
}());

<!---
keyai1/keyai1 is a ✨ special ✨ repository because its `README.md` (this file) appears on your GitHub profile.
You can click the Preview link to take a look at your changes.
--->
